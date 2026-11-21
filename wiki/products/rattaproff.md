---
type: product
title: "rattaproff"
product: rattaproff
project: null
created: 2026-04-06
updated: 2026-11-21
tags: [rattaproff, woocommerce, ecommerce, automation, google-indexing, gcp]
---

# rattaproff

WooCommerce multi-store automation and Google Indexing API management platform. Keeps a network of localised e-commerce stores stocked, priced, and discoverable in search.

## What it does

Rattaproff automates the full cycle from supplier catalogue ingestion to live product pages across **20 WooCommerce storefronts** (`SITE_CONFIGS` in `process.py`; original locales include rattaproff.ee, fahrrad7.de, velochic.fr, bicinegozio.it, bicirey.es, polkimet.fi, fietsgear.nl). It reconciles supplier feeds with merchandising decisions in Google Sheets, publishes price and availability updates to each storefront, and governs Google Indexing API submissions so fresh URLs reach search quickly without breaching the 200-requests/day quota.

## Target users

- E-commerce operations managers propagating supplier updates to multiple stores
- SEO and growth teams tracking indexing backlog and search visibility
- Platform engineers maintaining supplier integrations and indexing jobs

## Component repos

| Repo | Role |
|---|---|
| [[projects/rattaproff]] | Single repo — all automation logic, dashboards, Cloud Functions |

## Architecture summary

Python-based automation suite on GCP. Key components:
- **Supplier ingestion & diffing** (`suppliers.py`, `process.py`) — normalise catalogue data (`custom_packageunit` numeric sanitization), HUF→EUR pricing ([[concepts/huf-eur-pipeline-pricing]]), canonical HTML/typography/category diffing (`normalize_name`, `normalize_description`, `normalize_category_hierarchy`), image validation
- **Slim Google Sheet & sync safety** (`gsheet.py`, `gsheet_write.py`, `sheet_utils.py`) — 65-column `Sheet1` on `1tWrtr3hSpwW-jgf-KBVm8oaWMF0ax9-FfC2uJrwXNHU` (`RAW` writes, `78 MiB` cap, `__sync_status` + GCS fallback, [[concepts/gsheet-ground-truth-sync]])
- **WooCommerce publishing & Cloud SQL cache** (`woo.py`, `db_cache.py`) — per-site credentials, normalized category lookup, immediate `POST`/`PUT`/category write-through to Cloud SQL (sole source of product permalinks), bounded Cyrillic slug (`db_insert_error`) & non-JSON HTML `404` retries
- **Indexing backlog governance & draft guards** (`bigquery_backlog.py`, `indexing_api_utils.py`, `permalink_resolver.py`) — skip futile `HEAD`/`GET` probes on draft products, resolve publish permalinks post-`PUT`, queue via BigQuery backlog
- **Indexing dispatcher** — Cloud Function processing up to 200 URLs/day via Cloud Scheduler
- **Dashboards** — Streamlit (`dashboard_app.py`) and CLI (`indexing_dashboard.py`) for backlog visibility

## Current status

Operational across **20 storefronts**. **Slim Google Sheet migration (2026-11-21)** moved `Rattaproff Products` from the locked 118-column sheet (`1AGIUKToNIKJqIyQ5mUh-0bP3oKg1_Wlmqfnwmr-Kkiw`, `97.4 MB`, renamed `Rattaproff Products -- Old (do not use anymore)`) to a 65-column `RAW` spreadsheet (`1tWrtr3hSpwW-jgf-KBVm8oaWMF0ax9-FfC2uJrwXNHU`, `40,986` rows on `2026-11-21`, `81,045,617` bytes measured on `40,611` rows vs `78 MiB` cap), moved permalinks strictly to Cloud SQL, and purged legacy automated-translation code. **Woo sync comparison and draft indexing (2026-11-21)** cut the nightly robot batch run from ~43h (`154,296 s`) to **7.4h (`26,649 s`)** (`New: 2,596, Updated: 5,022, Deleted: 260`), eliminated HTML/typography/`27.5"` category churn, skipped draft redirect probes, and verified bounded Cyrillic slug & HTML `404` retries live. Docs: `docs/architecture/features/gsheet-ground-truth-sync.md`, `docs/architecture/features/gsheet-slim-migration.md`, `docs/architecture/features/woo-sync-comparison-and-draft-indexing.md`, `docs/architecture/category-url-export.md`.

## Related pages

- [[projects/rattaproff]]
- [[concepts/huf-eur-pipeline-pricing]]
- [[concepts/gsheet-ground-truth-sync]]
