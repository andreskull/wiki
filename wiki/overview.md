---
type: overview
title: "Wiki Overview"
created: 2026-04-06
updated: 2026-04-29
---

# Wiki Overview

Three products, one methodology, one wiki. Updated: 2026-04-29.

## Products

**[[products/finfluencer-trade]]** — Financial influencer accountability platform. Ingests podcast content, transcribes it, extracts stock picks, tracks prediction performance. Core repos: `gor_dagster` (data pipeline), `gor-blog` (public site + **`api/newsletter/`** ConvertKit tooling and Supabase→`GOR_NEWSLETTER_SUBS` import; private **`research/cramer/`** = Cramer export scripts, specs, SSRN admin), `finfluencer-tracker` (auth/landing layer). **`cramer-mad-money-research`** — public reproducibility kit + **SSRN** working paper [6643379](https://ssrn.com/abstract=6643379). **Planning docs** (growth plan, app MVP spec, post-MVP increments, app runbook) are linked from the product page — not from transient `gor_dagster/docs/features/<feature>/`. Pipeline is production-ready; **Dagster Cloud credit optimization** wrapped to `docs/architecture/features/` (2026-04-25); **8** feature folders remain in `gor_dagster/docs/features/`.

**[[products/rattaproff]]** — WooCommerce multi-store automation for a network of 7 e-commerce storefronts. Single repo. Handles supplier ingestion, pricing, publishing, and Google Indexing API quota management. Operational but docs sparse.

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
- [[concepts/spec-driven-development]] — the development methodology

## Open threads

- Botastico repos not yet indexed
- `rattaproff` docs sparse — sync when more content exists; **`finfluencer-tracker`** — run **`wiki sync finfluencer-tracker`** after changes to **`WIKI.md`** or **`docs/`** (excluding `docs/features/`)
- 8 gor_dagster feature folders in flight — wiki gains permanent `docs/architecture/features/` entries as each wraps up
