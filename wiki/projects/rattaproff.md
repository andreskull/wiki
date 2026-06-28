---
type: project
title: "rattaproff"
product: rattaproff
project: rattaproff
created: 2026-04-06
updated: 2026-06-28
tags: [woocommerce, ecommerce, automation, gcp, bigquery, indexing, supplier, python]
---

# rattaproff

Single-repo product. The full automation suite for [[products/rattaproff]] — supplier ingestion, multi-store WooCommerce publishing, and Google Indexing API governance.

## Product

[[products/rattaproff]]

## Purpose and role

Everything in one repo: supplier catalogue ingestion, reconciliation with Google Sheets merchandising decisions, WooCommerce publishing across **20 storefronts** (`SITE_CONFIGS`), BigQuery-backed indexing backlog, Cloud Function dispatcher, and monitoring dashboards.

## Tech stack

- **Language:** Python
- **Cloud:** GCP (BigQuery, Cloud Functions, Cloud Scheduler, Cloud Logging, GCS)
- **Storefronts:** WooCommerce (20 locales — see `SITE_CONFIGS` in `process.py`)
- **Data source:** Supplier feeds + Google Sheets ground truth (~42k SKUs)
- **Monitoring:** Streamlit dashboard + CLI tools

## Key modules

- `suppliers.py` / `process.py` — supplier ingestion and normalisation; HUF→EUR pricing and per-site recompute for Woo diffs ([[concepts/huf-eur-pipeline-pricing]])
- `permalink_resolver.py` — draft Woo URL → pretty permalink via HTTP redirect
- `woo.py` — WooCommerce publishing (per-site credentials via env)
- `db_cache.py` — Cloud SQL product cache; authoritative pretty permalinks post-backfill
- `storage.py` — GCS backups (`gsheet_*` CSV snapshots on every sheet sync)
- `bigquery_backlog.py` / `indexing_api_utils.py` — indexing queue and quota management
- `indexing_dispatcher_function/` — Cloud Function, 200 URLs/day limit
- `dashboard_app.py` / `indexing_dashboard.py` — monitoring UIs
- `gsheet.py` / `sheet_utils.py` — Google Sheets reconciliation

## Completed features

- **Permalink redirect resolution** — v1/v2 backfills complete; gsheet disaster recovery (June 2026). [permalink-redirect-resolution.md](file:///Users/andreskull/rattaproff/docs/architecture/features/permalink-redirect-resolution.md)
- **HUF/EUR per-site pricing** — [huf-eur-per-site-pricing.md](file:///Users/andreskull/rattaproff/docs/architecture/huf-eur-per-site-pricing.md)

## Docs structure

- `docs/architecture/` — durable feature and pricing notes
- `docs/features/` — active WIP specs only (`database-sync`, `per-site-huf-rate`)
- Legacy flat docs in `docs/` root

## Current status

Operational. **Permalink backfill:** complete (608k+ pretty cache permalinks; gsheet restored to 42,221 rows after failed sync truncated to 15k). **HUF/EUR:** default rate 350 HUF/EUR (`DEFAULT_HUF_EUR_RATE`); per-site overrides in `SITE_HUF_EUR_RATES` (empty = default).

Before robot execute after any gsheet incident: `scripts/summarize_change_plan.py` then `scripts/verify_gsheet_row_count.py`.

## Related pages

- [[products/rattaproff]]
- [[concepts/huf-eur-pipeline-pricing]]
- [[entities/bigquery]]
