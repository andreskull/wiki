---
type: project
title: "rattaproff"
product: rattaproff
project: rattaproff
created: 2026-04-06
updated: 2026-11-21
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

- `suppliers.py` / `process.py` — supplier ingestion and normalisation (`custom_packageunit` coerced to `float64`); HUF→EUR pricing and per-site recompute for Woo diffs ([[concepts/huf-eur-pipeline-pricing]]); `detect_updated_products` canonical HTML/typography/category diffing and `update_products_on_site` post-`PUT` publish indexing without draft redirect probes
- `permalink_resolver.py` — draft Woo URL → pretty permalink via HTTP redirect (invoked only when `resolve_draft=True`, skipped on draft writes)
- `woo.py` — WooCommerce publishing (per-site credentials via env), normalized `Woo.get_category_id_by_name` lookup, immediate `POST`/`PUT` & category write-through to Cloud SQL, bounded Cyrillic slug retry on `400 db_insert_error` (`_build_bounded_category_slug`), and non-JSON HTML `404` retry (`Woo._make_request`)
- `db_cache.py` — Cloud SQL product & category cache; sole home of product permalinks post slim migration (gallery-safe and non-empty-permalink-safe `upsert_product`)
- `storage.py` — GCS backups (`gsheet_*` CSV snapshots read with `SHEET1_TEXT_DTYPES` to preserve leading zeros)
- `bigquery_backlog.py` / `indexing_api_utils.py` — indexing queue and quota management
- `indexing_dispatcher_function/` — Cloud Function, 200 URLs/day limit
- `dashboard_app.py` / `indexing_dashboard.py` — monitoring UIs
- `gsheet.py` / `gsheet_write.py` / `sheet_utils.py` — Google Sheets reconciliation on the 65-column `SHEET1_COLUMNS` schema (`1tWrtr3hSpwW-jgf-KBVm8oaWMF0ax9-FfC2uJrwXNHU` via `RATTAPROFF_GSHEET_ID`), `RAW` values-only chunked writes (`__sync_status` + GCS fallback, [[concepts/gsheet-ground-truth-sync]]), and shared HTML/typography/category normalization helpers
- `scripts/migrate_slim_gsheet.py`, `scripts/prepare_slim_gsheet_cutover.py`, `scripts/verify_slim_robot_dryrun.py`, `scripts/verify_slim_robot_sheet_write.py`, `scripts/verify_woo_text_comparison.py` — migration, cutover (`--apply`, `--verify-prep`, `--verify-post-deploy`), and read-only comparison/dry-run verification gates
- `generate_category_urls.py` — live category permalinks from Woo REST slugs (not sheet-name transliteration); gitignored `category_urls_<domain>.csv` in repo root. [category-url-export.md](file:///Users/andreskull/rattaproff/docs/architecture/category-url-export.md)
- **Automated-translation purge:** Unused automated-translation packages, notebooks, and tests removed (`2026-11-21`); enforced by `tests/test_legacy_purge.py`.

## Architecture decisions

- **New 65-column slim spreadsheet (`RATTAPROFF_GSHEET_ID`), not in-place cleanup** — retired locked `1AGIUKToNIKJqIyQ5mUh-0bP3oKg1_Wlmqfnwmr-Kkiw` (`97,375,487` bytes, renamed `Rattaproff Products -- Old (do not use anymore)`) to drop revision history and `53` unused/permalink columns (`21` `permalink*`, `2` legacy spec, `7` empty, `23` supplier-intermediate). New `Sheet1` uses `RAW` values-only chunked writes (no formatting/apostrophe escaping) and rebuilt `et_de_fixed_categories`, `category_helper(dont change)` and `categories` pivot + 19 `VLOOKUP` columns, capped at `78 MiB` (`81,788,928` bytes). Permalinks live strictly in Cloud SQL.
- **No staging tab in live spreadsheet** — Google Sheets' 10M-cell limit is per file across all tabs (slim `Sheet1` is ~41k×65 ≈ 2.66M cells plus category helper/pivot tabs); duplicating data in-file would risk hard failures.
- **GCS before sheet & `__sync_status` marker** — `sync_sheet_with_ground_truth` snapshots intended ground truth to GCS before in-place chunked writes (`resize=False` + trim); `__sync_status` stays `in_progress` until complete, triggering GCS fallback on incomplete reads.
- **Category URLs come from Woo slugs** — WordPress `sanitize_title` owns the path. Guessing from translated sheet names 404s (`arrière` → `arri-re`). Exporter walks parent slugs from `/products/categories`.
- **Canonical normalization in comparison & category lookup only** — `normalize_name`, `normalize_description`, `normalize_category_hierarchy`, and `Woo.get_category_id_by_name` fold HTML/entities/typography/slashes/leading apostrophes for diffing and matching while keeping WordPress payloads canonical rather than rewriting ~38k live WooCommerce products.
- **Write-through Cloud SQL cache & draft redirect skip** — `Woo.add_products`/`update_product`/`create_category` update Cloud SQL and in-memory category caches immediately (`image_name` excluded so galleries stay intact); `Woo._effective_is_draft` sets `resolve_draft=False` on drafts (zero futile `HEAD`/`GET` 404 probes, existing pretty URLs kept in SQL), while publish updates read `last_updated_permalink` post-`PUT`.
- **Targeted `db_insert_error` & HTML `404` retries** — `Woo.create_category` retries with `_build_bounded_category_slug` (`<= 160` percent-encoded chars + `-{parent_id}`) only on WordPress `400 db_insert_error` so existing category slugs do not churn; `Woo._make_request` retries `404` (`15s`..`120s`) only when `body_is_non_json` and `retry=True` (leaving JSON `404`s and `get_products` fail-fast).

## Completed features

- **Permalink redirect resolution** — v1/v2 backfills complete; gsheet disaster recovery (June 2026). [permalink-redirect-resolution.md](file:///Users/andreskull/rattaproff/docs/architecture/features/permalink-redirect-resolution.md)
- **HUF/EUR per-site pricing** — [huf-eur-per-site-pricing.md](file:///Users/andreskull/rattaproff/docs/architecture/huf-eur-per-site-pricing.md)
- **GSheet ground-truth sync safety** — grow-only in-place writes, sync-status marker, GCS fallback (July 2026). [gsheet-ground-truth-sync.md](file:///Users/andreskull/rattaproff/docs/architecture/features/gsheet-ground-truth-sync.md)
- **Category URL export** — Woo-slug lists regenerated 2026-08-14. [category-url-export.md](file:///Users/andreskull/rattaproff/docs/architecture/category-url-export.md)
- **Slim Google Sheet migration** — 65-column `Sheet1` on `1tWrtr3hSpwW-jgf-KBVm8oaWMF0ax9-FfC2uJrwXNHU` (`RAW` writes, rebuilt `categories` tabs, `78 MiB` cap, automated-translation purge) (2026-11-21). [gsheet-slim-migration.md](file:///Users/andreskull/rattaproff/docs/architecture/features/gsheet-slim-migration.md)
- **Woo sync comparison and draft indexing** — canonical text/category comparison, Cloud SQL write-through caching, post-`PUT` publish indexing without draft redirect probes, and bounded-slug/HTML-`404` retries (2026-11-21). [woo-sync-comparison-and-draft-indexing.md](file:///Users/andreskull/rattaproff/docs/architecture/features/woo-sync-comparison-and-draft-indexing.md)

## Docs structure

- [WIKI.md](file:///Users/andreskull/rattaproff/WIKI.md) — repo wiki index
- `docs/architecture/` — durable feature and pricing notes (`features/` holds 4 feature wrapups)
- `docs/features/` — active/legacy WIP specs (`database-sync`, `per-site-huf-rate`)
- Legacy flat docs in `docs/` root

## Current status

Operational across all **20 storefronts**. **Spreadsheet:** active on slim `1tWrtr3hSpwW-jgf-KBVm8oaWMF0ax9-FfC2uJrwXNHU` (`RATTAPROFF_GSHEET_ID`, `40,986` rows on `2026-11-21`; prior snapshot measured `81,045,617` bytes against the `81,788,928`-byte / `78 MiB` cap, so adding large batches of new descriptions warrants size checks). **2026-11-21 live robot run (`rattaproff-robot-vm`, deployed from `e11ad8f`):** completed in **`26,649 s` (`444.2 m`)** (down from `154,296 s` / ~43h), **`New: 2,596, Updated: 5,022, Deleted: 260` (Grand Total `7,878`)**, zero unrecovered errors (`velosklad_bg` 4 duplicate Cyrillic categories retried cleanly via bounded slug `201`; `cykelpoint_dk` transient nginx HTML `404` recovered on attempt 3; `992` `URL_UPDATED` events & `0` draft warnings; `8` live `bicinegozio.it` URLs skipped by `validate_permalink` on `Exceeded 30 redirects`). Post-run 7-store `scripts/verify_woo_text_comparison.py` (`rattaproff_ee`, `fahrrad7_de`, `polkimet_fi`, `velosklad_bg`, `bikerai_ro`, `olabike_gr`, `tudobike_pt`) converged `7/7` stores to `db_after=(0,0,0)` and `live_after=(0,0,0)` (`total_eliminated=66`). **Git:** `main` / `origin/main` hold deployed code at `e11ad8f`; `feat/woo-sync-comparison-and-draft-indexing` is ahead at `00de043` with the latest wrapup docs awaiting promotion to `main`.

Operational checks: `scripts/verify_woo_text_comparison.py` (read-only DB + live Woo comparison), `scripts/prepare_slim_gsheet_cutover.py --verify-post-deploy`, and `python generate_category_urls.py` (`rp-3.11`). Before robot execute after any gsheet incident: `scripts/summarize_change_plan.py` then `scripts/verify_gsheet_row_count.py`.

## Related pages

- [[products/rattaproff]]
- [[concepts/huf-eur-pipeline-pricing]]
- [[concepts/gsheet-ground-truth-sync]]
- [[entities/bigquery]]
