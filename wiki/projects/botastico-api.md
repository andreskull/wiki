---
type: project
title: "botastico-api"
product: botastico
project: botastico-api
created: 2026-05-18
updated: 2026-11-12
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

Permanent feature write-up: [botastico-chat-image-attachments.md](file:///Users/andreskull/botastico-api/docs/architecture/features/botastico-chat-image-attachments.md).

## Key decisions

- **2026-11-12 — Switched to `ai-factory` (`1ec0a9f`) & closed GitHub Pages docs leak (`f48bbc3`):** Added [`AGENTS.md`](file:///Users/andreskull/botastico-api/AGENTS.md) (`Agent branch: none`, `CLAUDE.md` = `@AGENTS.md`), replacing `.cursor/rules/{git-branch,botastico}.mdc`. Preserves the single-branch `main` workflow where undeployed changes stay unstaged until in GCP production (`./deploy-api.sh` runs `gcloud run deploy --source .`, uploading unstaged edits; `/executor` saves per-leaf diffs to `docs/features/<name>/logs/<leaf>.patch`), and separates safe local `.venv/bin/pytest` unit modules from live `*_cloud_test.py` / `test_*_smoke.py` suites. Also deleted `.github/workflows/static.yml` and `gh-pages` (local + `origin/gh-pages`), which had been created in 2023 for `botastico-private-api-swagger-ui` and was publicly publishing all of `./docs` on `botastico.github.io`.
- **2026-05-18** — GCS previews: byte proxy for portal monitoring; Slack uses signed URLs from **`slack_chat_logs`** with explicit credentials on `generate_signed_url`, not IAM `signBlob` on the API/Run SA alone.

## Completed features (indexed)

| Completed | Feature | Doc |
|-----------|--------|-----|
| 2026-05-18 | Chat image attachments | [botastico-chat-image-attachments.md](file:///Users/andreskull/botastico-api/docs/architecture/features/botastico-chat-image-attachments.md) |

## Current status

Switched to [[projects/ai-factory]] (`Agent branch: none`) and retired public `gh-pages` docs workflow on **2026-11-12** (`1ec0a9f`, `f48bbc3`). Chat image attachments **in production** (2026-05-18). **`slack_chat_logs`** must be redeployed when attachment metadata or signing behavior changes.

## Repo entry point

[`WIKI.md`](file:///Users/andreskull/botastico-api/WIKI.md) and [`AGENTS.md`](file:///Users/andreskull/botastico-api/AGENTS.md) — sync and agent-rule anchors for this project.

Temporary specs: `docs/features/` (other in-flight features); wiki does not index those until `/wrapup`.

## Related pages

- [[products/botastico]]
- [[projects/botastico-portal]]
- [[projects/ai-factory]]
- [[wiki/entities/gcs]] — object storage pattern (also used for chat attachment audit bucket)
