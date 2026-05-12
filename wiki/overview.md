---
type: overview
title: "Wiki Overview"
created: 2026-04-06
updated: 2026-05-12
---

# Wiki Overview

Three products, one methodology, one wiki. Updated: 2026-05-12.

## Products

**[[products/finfluencer-trade]]** — Financial influencer accountability platform. Ingests podcast content, transcribes it, extracts stock picks, tracks prediction performance. Core repos: `gor_dagster` (data pipeline), `gor-blog` (public site + **`api/newsletter/`** ConvertKit tooling and Supabase→`GOR_NEWSLETTER_SUBS` import; private **`research/cramer/`** = Cramer export scripts, specs, SSRN admin), `finfluencer-tracker` (auth/landing layer). **`cramer-mad-money-research`** — public reproducibility kit + **SSRN** working paper [6643379](https://ssrn.com/abstract=6643379). **Planning docs** (growth plan, app MVP spec, post-MVP increments, app runbook) are linked from the product page — not from transient `gor_dagster/docs/features/<feature>/`. Pipeline is production-ready; **Dagster Cloud credit optimization** wrapped to `docs/architecture/features/` (2026-04-25); **8** feature folders remain in `gor_dagster/docs/features/`.

**[[products/rattaproff]]** — WooCommerce multi-store automation for a network of 7 e-commerce storefronts. Single repo. Handles supplier ingestion, **HUF→EUR pricing** (default 350 HUF/EUR; per-site overrides optional — [[concepts/huf-eur-pipeline-pricing]]), publishing, and Google Indexing API quota management. Repo architecture doc: `docs/architecture/huf-eur-per-site-pricing.md`.

**[[products/botastico]]** — Not yet indexed (repos not accessible during bootstrap). 7 repos. To be added in a future pass.

## Methodology

**[[projects/spec-driven-ai-coding]]** — The development process used across all projects. Spec → Plan → Execute → Wrapup. Provides global `/` commands for Cursor and Antigravity. The `/wrapup` command is the bridge between completed features and the wiki.

## Active development

`gor_dagster` has **8** in-progress feature folders under `docs/features/` (including `bigquery-cost-optimization`). **Dagster Cloud credit optimization** completed 2026-04-25 — see [[projects/gor_dagster]] (completed architecture features + file link to the repo doc). Temporary feature docs are not indexed here until `/wrapup`.

## Key concepts to know

- [[concepts/speaker-attribution]] — two-stage LLM pipeline for naming podcast speakers
- [[concepts/actionable-signal]] — the final output; a VIEW not a table; speaker-gated
- [[concepts/signal-performance]] — how picks become measured horizons; truncation + implicit flip on same instrument
- [[concepts/llm-config-registry]] — how multi-model experimentation is managed
- [[concepts/huf-eur-pipeline-pricing]] — rattaproff: HUF sheet vs per-site EUR recompute for Woo change detection

## Open threads

- Botastico repos not yet indexed
- `rattaproff` — architecture note on HUF/EUR pricing synced (May 2026); **`finfluencer-tracker`** — run **`wiki sync finfluencer-tracker`** after changes to **`WIKI.md`** or **`docs/`** (excluding `docs/features/`)
- 8 gor_dagster feature folders in flight — wiki gains permanent `docs/architecture/features/` entries as each wraps up
