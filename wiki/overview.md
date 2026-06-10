---
type: overview
title: "Wiki Overview"
created: 2026-04-06
updated: 2026-06-10
---

# Wiki Overview

Three products, one methodology, one wiki. Updated: 2026-06-10.

## Products

**[[products/finfluencer-trade]]** — Financial influencer accountability platform. Ingests podcast content, transcribes it, extracts stock picks, tracks prediction performance. Core repos: `gor_dagster` (data pipeline), `gor-blog` (public site + **`api/newsletter/`** ConvertKit tooling and Supabase→`GOR_NEWSLETTER_SUBS` import; private **`research/cramer/`** = Cramer export scripts, specs, SSRN admin), `finfluencer-tracker` (auth/landing layer). **`cramer-mad-money-research`** — public reproducibility kit + **SSRN** working paper [6643379](https://ssrn.com/abstract=6643379). **Planning docs** (growth plan, app MVP spec, post-MVP increments, app runbook) are linked from the product page — not from transient `gor_dagster/docs/features/<feature>/`. Pipeline is production-ready; **resolution pipeline efficiency** wrapped **2026-06-10** ([[concepts/resolution-pipeline-efficiency]]); **BigQuery cost optimization** wrapped **2026-05-17** ([[projects/gor_dagster]]). Several feature folders remain under `gor_dagster/docs/features/`.

**[[products/rattaproff]]** — WooCommerce multi-store automation for a network of 7 e-commerce storefronts. Single repo. Handles supplier ingestion, **HUF→EUR pricing** (default 350 HUF/EUR; per-site overrides optional — [[concepts/huf-eur-pipeline-pricing]]), publishing, and Google Indexing API quota management. Repo architecture doc: `docs/architecture/huf-eur-per-site-pricing.md`.

**[[products/botastico]]** — First project page: [[projects/botastico-api]] (2026-05-18): Cloud Run API, chat image attachments, Pub/Sub → `slack_chat_logs`. Other botastico repos still light in wiki.

## Methodology

**[[projects/spec-driven-ai-coding]]** — The development process used across all projects. Spec → Plan → Execute → Wrapup. Provides global `/` commands for Cursor and Antigravity. The `/wrapup` command is the bridge between completed features and the wiki.

## Active development

`gor_dagster` has in-progress feature folders under `docs/features/` (recursive LLM extraction, show-profile access, batch integration, etc.). **Resolution pipeline efficiency** completed **2026-06-10** — see [[concepts/resolution-pipeline-efficiency]]. Temporary feature docs are not indexed here until `/wrapup`.

## Key concepts to know

- [[concepts/speaker-attribution]] — two-stage LLM pipeline for naming podcast speakers
- [[concepts/actionable-signal]] — the final output; a VIEW not a table; speaker-gated
- [[concepts/signal-performance]] — how picks become measured horizons; truncation + implicit flip on same instrument
- [[concepts/llm-config-registry]] — how multi-model experimentation is managed
- [[concepts/huf-eur-pipeline-pricing]] — rattaproff: HUF sheet vs per-site EUR recompute for Woo change detection
- [[concepts/resolution-pipeline-efficiency]] — JW matcher, re-attempt union, backlog sweeps for instrument/speaker resolution
- [[concepts/onboarding-new-podcast-source]] — gor_dagster: add a podcast RSS end-to-end (regex + SI sensor gates)

## Open threads

- Botastico: **`botastico-api`** indexed; portal/script/stripe/etc. still thin in wiki
- `rattaproff` — architecture note on HUF/EUR pricing synced (May 2026); **`finfluencer-tracker`** — run **`wiki sync finfluencer-tracker`** after changes to **`WIKI.md`** or **`docs/`** (excluding `docs/features/`)
- **Four** `gor_dagster` feature folders in flight — wiki gains permanent `docs/architecture/features/` entries as each wraps up
