---
type: overview
title: "Wiki Overview"
created: 2026-04-06
updated: 2026-07-11
---

# Wiki Overview

Three products, one methodology, one wiki. Updated: 2026-07-11.

## Products

**[[products/finfluencer-trade]]** — Financial influencer accountability platform. Ingests podcast content, transcribes it, extracts stock picks, tracks prediction performance. Core repos: `gor_dagster` (data pipeline), `gor-blog` (public site + **`api/newsletter/`** ConvertKit tooling and Supabase→`GOR_NEWSLETTER_SUBS` import; private **`research/cramer/`** = Cramer export scripts, specs, SSRN admin), **`finfluencer-tracker`** (Vercel app + landing — **CNBC IPO scoreboard live 2026-07-11** at `/cnbc-ipo`; public conversion funnel **2026-07-10**; see [[projects/finfluencer-tracker]]). **`cramer-mad-money-research`** — public reproducibility kit + **SSRN** working paper [6643379](https://ssrn.com/abstract=6643379). **Planning docs** (growth plan, app MVP spec, post-MVP increments, app runbook) are linked from the product page — not from transient `gor_dagster/docs/features/<feature>/`. Pipeline is production-ready; **CNBC IPO scoreboard** wrapped **2026-07-11** ([[projects/gor_dagster]]); **recursive LLM extraction** wrapped **2026-07-01** ([[concepts/speaker-attribution]], [[concepts/llm-config-registry]]); **proof-segment speaker resolution** wrapped **2026-07-01** ([[concepts/proof-segment-speaker-resolution]]); **Motley Fool Hidden Gems** wrapped **2026-07-01**; **SPY canonical FIGI consolidation** wrapped **2026-06-30**; **transcript hydration performance optimization** wrapped **2026-06-15**; **resolution pipeline efficiency** wrapped **2026-06-10** ([[concepts/resolution-pipeline-efficiency]]); **curation learning** (instrument resolver P1/P2/P3) wrapped **2026-07-07** ([[concepts/curation-learning]]); **BigQuery cost optimization** wrapped **2026-05-17**. Active feature folders: batch-integration (Phase 5 = recursive SI wave batch), post-cutoff IPO resolution, others.

**[[products/rattaproff]]** — WooCommerce multi-store automation for a network of 7 e-commerce storefronts. Single repo. Handles supplier ingestion, **HUF→EUR pricing** (default 350 HUF/EUR; per-site overrides optional — [[concepts/huf-eur-pipeline-pricing]]), publishing, and Google Indexing API quota management. Repo architecture doc: `docs/architecture/huf-eur-per-site-pricing.md`.

**[[products/botastico]]** — First project page: [[projects/botastico-api]] (2026-05-18): Cloud Run API, chat image attachments, Pub/Sub → `slack_chat_logs`. Other botastico repos still light in wiki.

## Methodology

**[[projects/spec-driven-ai-coding]]** — The development process used across all projects. Spec → Plan → Execute → Wrapup. Provides global `/` commands for Cursor and Antigravity. The `/wrapup` command is the bridge between completed features and the wiki.

## Active development

`gor_dagster` has in-progress feature folders under `docs/features/` (batch-integration Phase 5 recursive SI wave batch, post-cutoff IPO resolution, social share previews, etc.). **CNBC IPO scoreboard** completed **2026-07-11** — live `/cnbc-ipo`, social/blog deferred; see [[projects/gor_dagster]] and [[projects/finfluencer-tracker]]. **Recursive LLM extraction** (interactive path) completed **2026-07-01** — see [[projects/gor_dagster]] and [[concepts/speaker-attribution]]. **Transcript hydration performance optimization** completed **2026-06-15**. **Resolution pipeline efficiency** completed **2026-06-10** — see [[concepts/resolution-pipeline-efficiency]]. **Curation learning** (instrument resolver) completed **2026-07-07** — see [[concepts/curation-learning]]. **finfluencer-tracker landing conversion funnel** wrapped **2026-07-10** — see [[projects/finfluencer-tracker]]. Temporary feature docs are not indexed here until `/wrapup`.

## Key concepts to know

- [[concepts/speaker-attribution]] — two-stage LLM pipeline; production recursive SI `si-gem31fl-recursive` (2026-07-01); Stage 2 hydration O(U log W + W) with FR-6 membership (2026-06-15)
- [[concepts/actionable-signal]] — the final output; a VIEW not a table; proof-segment speaker-gated
- [[concepts/proof-segment-speaker-resolution]] — evidence quote speakers → Finfluencer; display_name at sync (2026-07-01)
- [[concepts/signal-performance]] — how picks become measured horizons; truncation + implicit flip; SPY benchmark at `BBG000BDTBL9`; as-of exit pricing (2026-06-30)
- [[concepts/llm-config-registry]] — multi-model experimentation; production `si-gem31fl-recursive` / `fe-gem31fl-recursive`
- [[concepts/huf-eur-pipeline-pricing]] — rattaproff: HUF sheet vs per-site EUR recompute for Woo change detection
- [[concepts/resolution-pipeline-efficiency]] — JW matcher, re-attempt union, backlog sweeps for instrument/speaker resolution
- [[concepts/curation-learning]] — fund-noise similarity, unique-ticker bar 0.85, Stage 0.75 promotion (2026-07-07)
- [[concepts/onboarding-new-podcast-source]] — gor_dagster: add a podcast RSS end-to-end (regex gate; SI auto-discovers `podcast_rss` since 2026-07-01)

## Open threads

- Botastico: **`botastico-api`** indexed; portal/script/stripe/etc. still thin in wiki
- `rattaproff` — architecture note on HUF/EUR pricing synced (May 2026)
- **Four** `gor_dagster` feature folders in flight (batch-integration, finfluencer-and-show-profiles, social-share-previews, post-cutoff-ipo-resolution) — wiki gains permanent `docs/architecture/features/` entries as each wraps up
- **finfluencer-tracker:** Stripe Live wallet Dashboard confirmation optional follow-up; magic-link deliverability deferred
