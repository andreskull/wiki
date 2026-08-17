---
type: product
title: "rattaproff"
product: rattaproff
project: null
created: 2026-04-06
updated: 2026-08-14
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
- **Supplier ingestion** (`suppliers.py`, `process.py`) — normalise catalogue data, HUF→EUR pricing ([[concepts/huf-eur-pipeline-pricing]]), image validation
- **WooCommerce publishing** (`woo.py`) — per-site credentials, create/update/delete operations
- **Indexing backlog governance** (`bigquery_backlog.py`, `indexing_api_utils.py`) — BigQuery-backed queue, quota enforcement
- **Indexing dispatcher** — Cloud Function processing up to 200 URLs/day via Cloud Scheduler
- **Dashboards** — Streamlit (`dashboard_app.py`) and CLI (`indexing_dashboard.py`) for backlog visibility

## Current status

Operational. Core supplier-to-storefront pipeline running. **GSheet ground-truth sync** hardened July 2026 — grow-only writes, `__sync_status` marker, GCS fallback ([[concepts/gsheet-ground-truth-sync]]). **Category URL export** (2026-08-14) — live Woo slugs, not guessed sheet names; gitignored `category_urls_*.csv` in the repo root. Docs: `docs/architecture/features/gsheet-ground-truth-sync.md`, `docs/architecture/category-url-export.md`.

## Related pages

- [[projects/rattaproff]]
- [[concepts/huf-eur-pipeline-pricing]]
- [[concepts/gsheet-ground-truth-sync]]
