---
type: project
title: "rattaproff"
product: rattaproff
project: rattaproff
created: 2026-04-06
updated: 2026-08-14
tags: [woocommerce, ecommerce, automation, gcp, bigquery, indexing, supplier, python, google-sheets]
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
- **Data source:** Supplier feeds + Google Sheets ground truth (~40k SKUs)
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
- `gsheet.py` / `sheet_utils.py` — Google Sheets reconciliation; safe ground-truth writes ([[concepts/gsheet-ground-truth-sync]])
- `generate_category_urls.py` — live category permalinks from Woo REST slugs (not sheet-name transliteration); gitignored `category_urls_<domain>.csv` in repo root. [category-url-export.md](file:///Users/andreskull/rattaproff/docs/architecture/category-url-export.md)

## Architecture decisions

- **No staging tab in live spreadsheet** — Google Sheets 10M-cell limit is per file across all tabs; ~40k×118 cols is already ~4.8M cells; duplicating data in-file would risk hard failures.
- **GCS before sheet** — `sync_sheet_with_ground_truth` snapshots intended ground truth to GCS before writing the sheet; readers fall back if sync-status is incomplete.
- **`__sync_status` marker** — tiny auto-created tab; `in_progress` during write, `complete` only after all chunks + trim succeed.
- **Category URLs come from Woo slugs** — WordPress `sanitize_title` owns the path. Guessing from translated sheet names 404s (`arrière` → `arri-re`). Exporter walks parent slugs from `/products/categories`.

## Completed features

- **Permalink redirect resolution** — v1/v2 backfills complete; gsheet disaster recovery (June 2026). [permalink-redirect-resolution.md](file:///Users/andreskull/rattaproff/docs/architecture/features/permalink-redirect-resolution.md)
- **HUF/EUR per-site pricing** — [huf-eur-per-site-pricing.md](file:///Users/andreskull/rattaproff/docs/architecture/huf-eur-per-site-pricing.md)
- **GSheet ground-truth sync safety** — grow-only in-place writes, sync-status marker, GCS fallback (July 2026). [gsheet-ground-truth-sync.md](file:///Users/andreskull/rattaproff/docs/architecture/features/gsheet-ground-truth-sync.md)
- **Category URL export** — Woo-slug lists regenerated 2026-08-14. [category-url-export.md](file:///Users/andreskull/rattaproff/docs/architecture/category-url-export.md)

## Docs structure

- `docs/architecture/` — durable feature and pricing notes
- `docs/features/` — active WIP specs only (`database-sync`, `per-site-huf-rate`)
- Legacy flat docs in `docs/` root

## Current status

Operational. **Gsheet:** restored to ~40k rows (July 2026); robot write path hardened — no manual `__sync_status` setup needed (created on first post-deploy write). **Permalink backfill:** complete (608k+ pretty cache permalinks). **HUF/EUR:** default rate 350 HUF/EUR (`DEFAULT_HUF_EUR_RATE`); per-site overrides in `SITE_HUF_EUR_RATES` (empty = default). **Category URL lists:** regenerate with `python generate_category_urls.py` (env `rp-3.11`); files at repo root `category_urls_*.csv` (gitignored). Includes **tudobike.pt** (`--domain tudobike.pt`).

Before robot execute after any gsheet incident: `scripts/summarize_change_plan.py` then `scripts/verify_gsheet_row_count.py`.

## Related pages

- [[products/rattaproff]]
- [[concepts/huf-eur-pipeline-pricing]]
- [[concepts/gsheet-ground-truth-sync]]
- [[entities/bigquery]]
