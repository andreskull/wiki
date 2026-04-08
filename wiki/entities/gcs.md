---
type: entity
title: "Google Cloud Storage"
product: null
project: null
created: 2026-04-06
updated: 2026-04-06
tags: [gcp, storage, gcs, media, transcripts]
---

# Google Cloud Storage (GCS)

Object storage on Google Cloud. Used for large blobs — audio, images, JSON transcripts — that do not belong in BigQuery row storage.

## How it's used

- **finfluencer.trade / gor_dagster:** Two buckets matter most: `gor-media-prod` (episode audio, source images; often from `GCS_BUCKET_NAME`) and `gor-stt-transcripts` (STT raw/unified/hydrated JSON — **hardcoded** in `definitions.py`, not the same env var as general media).
- **rattaproff:** Product assets and automation artefacts per store (see project docs).

## Projects using it

- [[projects/gor_dagster]]
- [[projects/rattaproff]]

## Related pages

- [[entities/bigquery]]
- [[concepts/speaker-attribution]]
