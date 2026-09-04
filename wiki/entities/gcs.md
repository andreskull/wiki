---
type: entity
title: "Google Cloud Storage"
product: null
project: null
created: 2026-04-06
updated: 2026-09-04
tags: [gcp, storage, gcs, media, transcripts]
---

# Google Cloud Storage (GCS)

Object storage on Google Cloud. Used for large blobs — audio, images, JSON transcripts — that do not belong in BigQuery row storage.

## How it's used

- **finfluencer.trade / gor_dagster:** Two buckets matter most: `gor-media-prod` (episode audio, source images; often from `GCS_BUCKET_NAME`) and `gor-stt-transcripts` (STT raw/unified/hydrated JSON — **hardcoded** in `definitions.py`, not the same env var as general media). Under `gor-media-prod`: `outreach/charts/` (LinkedIn MP4s) and **`marketing/google-ads/`** (PMax creative archive — write-once runs + `live.json`, **2026-09-04**). See [[concepts/google-ads-creative-assets]].
- **rattaproff:** Product assets and automation artefacts per store (see project docs).
- **botastico / botastico-api:** Dedicated **`CHAT_ATTACHMENTS_BUCKET_NAME`** for processed chat-image audit blobs (separate from customer KB assets and chat-log JSON). Signed URL or proxy patterns differ by surface (portal vs Slack); see [[projects/botastico-api]].

## Projects using it

- [[projects/gor_dagster]]
- [[projects/rattaproff]]
- [[projects/botastico-api]]

## Related pages

- [[entities/bigquery]]
- [[concepts/speaker-attribution]]
- [[concepts/google-ads-creative-assets]]
- [[concepts/linkedin-outreach]]
- [[projects/botastico-api]]
