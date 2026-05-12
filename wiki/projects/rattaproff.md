---
type: project
title: "rattaproff"
product: rattaproff
project: rattaproff
created: 2026-04-06
updated: 2026-05-12
tags: [woocommerce, ecommerce, automation, gcp, bigquery, indexing, supplier, python]
---

# rattaproff

Single-repo product. The full automation suite for [[products/rattaproff]] — supplier ingestion, multi-store WooCommerce publishing, and Google Indexing API governance.

## Product

[[products/rattaproff]]

## Purpose and role

Everything in one repo: supplier catalogue ingestion, reconciliation with Google Sheets merchandising decisions, WooCommerce publishing across 7 storefronts, BigQuery-backed indexing backlog, Cloud Function dispatcher, and monitoring dashboards.

## Tech stack

- **Language:** Python
- **Cloud:** GCP (BigQuery, Cloud Functions, Cloud Scheduler, Cloud Logging)
- **Storefronts:** WooCommerce (7 locales: .ee, .de, .fr, .it, .es, .fi, .nl)
- **Data source:** Supplier feeds + Google Sheets overrides
- **Monitoring:** Streamlit dashboard + CLI tools

## Key modules

- `suppliers.py` / `process.py` — supplier ingestion and normalisation; HUF→EUR pricing and per-site recompute for Woo diffs ([[concepts/huf-eur-pipeline-pricing]])
- `woo.py` — WooCommerce publishing (per-site credentials via env)
- `bigquery_backlog.py` / `indexing_api_utils.py` — indexing queue and quota management
- `indexing_dispatcher_function/` — Cloud Function, 200 URLs/day limit
- `dashboard_app.py` / `indexing_dashboard.py` — monitoring UIs
- `gsheet.py` / `sheet_utils.py` — Google Sheets reconciliation

## Docs structure

Scaffold exists (`docs/architecture/`, `docs/operations/`, `docs/schemas/` — empty). Some legacy flat docs in `docs/` root (`FINAL_DEPLOYMENT_GUIDE.md`, `QUICK_REFERENCE.md`, etc.). One active feature: `docs/features/database-sync/`.

Has `.ai-rules/` with `product.md`, `tech.md`, `structure.md` populated.

## Current status

Operational. **HUF/EUR:** default rate is 350 HUF per EUR repo-wide (`DEFAULT_HUF_EUR_RATE`); per-site overrides live in `SITE_HUF_EUR_RATES` (currently empty = all sites use default). Architecture note: [huf-eur-per-site-pricing.md](file:///Users/andreskull/rattaproff/docs/architecture/huf-eur-per-site-pricing.md). Wiki concept: [[concepts/huf-eur-pipeline-pricing]]. Documentation elsewhere still sparse — `/wrapup` on `database-sync` when complete.

## Related pages

- [[products/rattaproff]]
- [[concepts/huf-eur-pipeline-pricing]]
- [[entities/bigquery]]
