---
type: project
title: "botastico-api"
product: botastico
project: botastico-api
created: 2026-05-18
updated: 2026-11-19
tags: [botastico, flask, cloud-run, firestore, gcs, rag, chat]
---

# botastico-api

Python Flask API for [[products/botastico]]: agents, chat with RAG (Postgres/pgvector), KB-related endpoints, organizations, Stripe-adjacent routes, and v1 REST. Runs on **Google Cloud Run**. Data: **Firestore**, **GCS**, **Postgres**; **Pub/Sub** topic `interaction-log` for downstream audit/notifications.

## Product

[[products/botastico]]

## Purpose and role

Primary backend for chat execution, agent configuration, and retrieval-augmented answers. Embeds and widget traffic reach it via **`botastico-portal`** (Next.js API routes with server-side credentials). Interaction logging feeds monitoring and Slack via Pub/Sub consumers outside this repo.

## Tech stack (high level)

- **Runtime:** Python 3.11, Flask
- **Deploy:** Cloud Run (`deploy-api.sh` — **manual only**; see [[products/botastico]] § Deployment Rules)
- **Data:** Firestore, GCS (`CUSTOMERS_DATA_BUCKET_NAME`, `CHAT_LOGS_BUCKET_NAME`, `CHAT_ATTACHMENTS_BUCKET_NAME`, …), Postgres for embeddings search

## Architecture summary

- **Chat:** Text + optional **image attachments** (OpenAI vision, agent-flag gated). Upload is two-phase (attachment IDs, then prompt). Logs and Pub/Sub carry **metadata only** (no base64 in persisted payloads).
- **RAG:** Text-only retrieval against pgvector; chat images are not indexed into the durable KB.
- **Monitoring:** Portal ([[projects/botastico-portal]]) proxies image previews from the API where signed URLs from default SA credentials are unavailable (e.g. Cloud Run `signBlob`).
- **Slack audit:** Cloud Function **`slack_chat_logs`** in repo **`botastico`** (`slack_chat_logs/`) subscribes to `interaction-log`, posts customer prompt + attachment **image blocks** (short-lived v4 signed URLs using runtime `access_token`).
- **File ingestion & shared utils:** `file_processor/` runs as a separate Cloud Run service (`file-processor-{staging-west1,production-west1}`) and Cloud Run Jobs for document chunking/ingestion; `python_utils/` is a git submodule (`https://github.com/botastico/python_utils.git`) copied into `file_processor/python_utils` during image builds by `file_processor/deploy-main.sh`.

Permanent feature write-ups:
- [gpt-5-1-retirement.md](file:///Users/andreskull/botastico-api/docs/architecture/features/gpt-5-1-retirement.md)
- [deprecated-openai-models-removal.md](file:///Users/andreskull/botastico-api/docs/architecture/features/deprecated-openai-models-removal.md)
- [botastico-chat-image-attachments.md](file:///Users/andreskull/botastico-api/docs/architecture/features/botastico-chat-image-attachments.md)

## Key decisions

- **2026-11-19 — GPT-5.1 retirement & `gpt-6-luna` rollout (`botastico-api-production-west1-00089-tbp`, `20f09d1`):** Added `gpt-6-luna` (`MAX_TOKENS: 16384`, `IA_CREDITS: 1` from \$0.10 input / \$0.50 output per 1M tokens, OpenAI vision capability matching `gpt-5.4-mini`) with dedicated `_build_openai_streaming_params` / `_build_openai_non_streaming_params` sending `max_completion_tokens` and `reasoning_effort='none'` without `verbosity` (and `tools` + `tool_choice='auto'` on non-streaming only) plus `_LazyTokenHelper` to avoid import-time `tiktoken` network calls. Rolled out in two stages (Revision A -> Firestore migration -> Revision B) to avoid customer downtime: soft-deleted 18 churned DAO (`17JMLFaZfInEkuz0clD5`) agents, migrated all live customer agents (`19` staging, `49` production) and Firestore subscription plans (`4` staging, `7` production) to `gpt-6-luna` (`0` live agents off `gpt-6-luna`, `0` plans pending), then removed `gpt-5.1`, `gpt-5.4-nano`, `gpt-5-nano`, `gpt-5-mini`, `claude-3-haiku-20240307`, `claude-3-5-haiku-20241022`, and `claude-3-7-sonnet-20250219` from `SUPPORTED_MODELS`, `MAX_TOKENS`, and `IA_CREDITS` (narrowing the active catalog to `gpt-4o`, `gpt-4o-mini`, `gpt-5.4-mini`, `gpt-6-luna`, `claude-haiku-4-5-20251001`, and `claude-sonnet-4-6`, and `IMAGE_MODEL_CAPABILITIES` to `gpt-4o`, `gpt-4o-mini`, `gpt-5.4-mini`, and `gpt-6-luna`). Removed completed one-time migration scripts (`scripts/migrate_gpt51_agents.py`, `scripts/migrate_deprecated_openai_models.py`, `scripts/migrate_deprecated_claude_models.py`, `scripts/migrate_gpt5_nano_mini_agents.py`, `scripts/audit_retired_claude_agents.py`) via `/cleanup`.
- **2026-11-15 — Deprecated OpenAI models removed & `o3-mini` mapped to `gpt-5.4-mini` (`botastico-api-production-west1-00087`):** Removed `gpt-3.5-turbo*`, `gpt-4-turbo*` / preview variants, and `o3-mini` from `SUPPORTED_MODELS`, `MAX_TOKENS`, and `IA_CREDITS` after staging and production dry-runs confirmed 0 remaining in-scope agents and plans (`_agent_in_migration_scope` skips deleted agents and soft-deleted organizations). Mapped `o3-mini` to `gpt-5.4-mini` (3 IA credits vs 7 previously, avoiding expensive `o3`), gated production `--apply` on a live `https://developers.openai.com/api/docs/deprecations` alias check (`api/openai_deprecations_check.py`), added `claude-haiku-4-5-20251001` and `claude-sonnet-4-6` to the catalog, and kept `gpt-5.1` (`GPT_5_1_RETIREMENT_*`) and `gpt-5-nano`/`gpt-5-mini` (`GPT_5_NANO_MINI_RETIREMENT_*`) on separate retirement tracks.
- **2026-11-12 — Switched to `ai-factory` (`1ec0a9f`) & closed GitHub Pages docs leak (`f48bbc3`):** Added [`AGENTS.md`](file:///Users/andreskull/botastico-api/AGENTS.md) (`Agent branch: none`, `CLAUDE.md` = `@AGENTS.md`), replacing `.cursor/rules/{git-branch,botastico}.mdc`. Preserves the single-branch `main` workflow where undeployed changes stay unstaged until in GCP production (`./deploy-api.sh` runs `gcloud run deploy --source .`, uploading unstaged edits; `/executor` saves per-leaf diffs to `docs/features/<name>/logs/<leaf>.patch`), and separates safe local `.venv/bin/pytest` unit modules from live `*_cloud_test.py` / `test_*_smoke.py` suites. Also deleted `.github/workflows/static.yml` and `gh-pages` (local + `origin/gh-pages`), which had been created in 2023 for `botastico-private-api-swagger-ui` and was publicly publishing all of `./docs` on `botastico.github.io`.
- **2026-05-18** — GCS previews: byte proxy for portal monitoring; Slack uses signed URLs from **`slack_chat_logs`** with explicit credentials on `generate_signed_url`, not IAM `signBlob` on the API/Run SA alone.

## Completed features (indexed)

| Completed | Feature | Doc |
|-----------|--------|-----|
| 2026-11-19 | GPT-5.1 retirement and `gpt-6-luna` rollout | [gpt-5-1-retirement.md](file:///Users/andreskull/botastico-api/docs/architecture/features/gpt-5-1-retirement.md) |
| 2026-11-15 | Deprecated OpenAI models removal | [deprecated-openai-models-removal.md](file:///Users/andreskull/botastico-api/docs/architecture/features/deprecated-openai-models-removal.md) |
| 2026-05-18 | Chat image attachments | [botastico-chat-image-attachments.md](file:///Users/andreskull/botastico-api/docs/architecture/features/botastico-chat-image-attachments.md) |

## Current status

**GPT-5.1 retirement and `gpt-6-luna` rollout** wrapped and verified live in production on **2026-11-19** (`botastico-api-production-west1-00089-tbp`, `20f09d1` on `main`; all 49 live production agents and 7 plans migrated to `gpt-6-luna` after deleting 18 churned DAO agents; 7 retired GPT-5 and Claude 3 models removed from the catalog; live Botastico, Opiq, and Blurby interactions verified). **Deprecated OpenAI models removal** wrapped **2026-11-15** (`botastico-api-production-west1-00087` + `botastico-portal` `main`). Switched to [[projects/ai-factory]] (`Agent branch: none`) and retired public `gh-pages` docs workflow on **2026-11-12** (`1ec0a9f`, `f48bbc3`). Chat image attachments **in production** (2026-05-18). **`slack_chat_logs`** must be redeployed when attachment metadata or signing behavior changes.

## Repo entry point

[`WIKI.md`](file:///Users/andreskull/botastico-api/WIKI.md) and [`AGENTS.md`](file:///Users/andreskull/botastico-api/AGENTS.md) — sync and agent-rule anchors for this project.

Temporary specs: `docs/features/` (other in-flight features); wiki does not index those until `/wrapup`.

## Related pages

- [[products/botastico]]
- [[projects/botastico-portal]]
- [[projects/ai-factory]]
- [[wiki/entities/gcs]] — object storage pattern (also used for chat attachment audit bucket)
