---
type: project
title: "botastico-api"
product: botastico
project: botastico-api
created: 2026-05-18
updated: 2026-05-18
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
- **Monitoring:** Portal proxies image previews from the API where signed URLs from default SA credentials are unavailable (e.g. Cloud Run `signBlob`).
- **Slack audit:** Cloud Function **`slack_chat_logs`** in repo **`botastico`** (`slack_chat_logs/`) subscribes to `interaction-log`, posts customer prompt + attachment **image blocks** (short-lived v4 signed URLs using runtime `access_token`).

Permanent feature write-up: [botastico-chat-image-attachments.md](file:///Users/andreskull/botastico-api/docs/architecture/features/botastico-chat-image-attachments.md).

## Key decisions

- **2026-05-18** — GCS previews: byte proxy for portal monitoring; Slack uses signed URLs from **`slack_chat_logs`** with explicit credentials on `generate_signed_url`, not IAM `signBlob` on the API/Run SA alone.

## Completed features (indexed)

| Completed | Feature | Doc |
|-----------|--------|-----|
| 2026-05-18 | Chat image attachments | [botastico-chat-image-attachments.md](file:///Users/andreskull/botastico-api/docs/architecture/features/botastico-chat-image-attachments.md) |

## Current status

Chat image attachments **in production** (2026-05-18). **`slack_chat_logs`** must be redeployed when attachment metadata or signing behavior changes.

## Repo entry point

[`WIKI.md`](file:///Users/andreskull/botastico-api/WIKI.md) — sync anchor for this project.

Temporary specs: `docs/features/` (other in-flight features); wiki does not index those until `/wrapup`.

## Related pages

- [[products/botastico]]
- [[projects/spec-driven-ai-coding]]
- [[wiki/entities/gcs]] — object storage pattern (also used for chat attachment audit bucket)
