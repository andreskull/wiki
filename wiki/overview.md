---
type: overview
title: "Wiki Overview"
created: 2026-04-06
updated: 2026-07-27
---

# Wiki Overview

Three products, one methodology, one wiki. Updated: 2026-07-27.

## Products

**[[products/finfluencer-trade]]** — Financial influencer accountability platform. Ingests podcast content, transcribes it, extracts stock picks, tracks prediction performance. Core repos: `gor_dagster` (data pipeline), `gor-blog` (public site + **`api/newsletter/`**; private **`research/cramer/`**), **`finfluencer-tracker`** (Vercel app + landing — **subscription entitlement SSOT live 2026-07-27** [[concepts/subscription-entitlement-ssot]]; **CNBC IPO scoreboard** `/cnbc-ipo` **2026-07-11**; public conversion funnel **2026-07-10**; see [[projects/finfluencer-tracker]]). **`cramer-mad-money-research`** — public kit + SSRN [6643379](https://ssrn.com/abstract=6643379). Pipeline production-ready; **LinkedIn enrichment** **2026-07-23** ([[concepts/linkedin-enrichment]]); recursive SI/FE, proof-segment resolution, curation learning, and other pipeline wraps as before. Active `gor_dagster` feature folders under `docs/features/` (batch-integration, post-cutoff IPO, others).

**[[products/rattaproff]]** — WooCommerce multi-store automation for a network of 7 e-commerce storefronts. Single repo. Handles supplier ingestion, **HUF→EUR pricing** (default 350 HUF/EUR; per-site overrides optional — [[concepts/huf-eur-pipeline-pricing]]), publishing, and Google Indexing API quota management. **GSheet ground-truth sync safety** (July 2026) — grow-only robot writes, `__sync_status` marker, GCS fallback — [[concepts/gsheet-ground-truth-sync]]. Repo docs: `docs/architecture/huf-eur-per-site-pricing.md`, `docs/architecture/features/gsheet-ground-truth-sync.md`.

**[[products/botastico]]** — [[projects/botastico-api]] (2026-05-18): Cloud Run API, chat image attachments, Pub/Sub → `slack_chat_logs`. **[[projects/botastico]]** (2026-07-15): GCP LB SSL for `chatapps`/`assets` widget delivery; July 2026 cert incident fixed; renewal-failure monitoring — [[concepts/botastico-ssl-certificates]]. Other botastico repos still light in wiki.

## Methodology

**[[projects/spec-driven-ai-coding]]** — The development process used across all projects. Spec → Plan → Execute → Wrapup. Provides global `/` commands for Cursor and Antigravity. The `/wrapup` command is the bridge between completed features and the wiki.

## Active development

`gor_dagster` has in-progress feature folders under `docs/features/` (batch-integration, post-cutoff IPO resolution, etc.). **finfluencer-tracker subscription entitlement SSOT** wrapped **2026-07-27** — see [[concepts/subscription-entitlement-ssot]]. **LinkedIn enrichment** **2026-07-23** ([[concepts/linkedin-enrichment]]). **CNBC IPO scoreboard** **2026-07-11**. Landing conversion funnel **2026-07-10**. Temporary feature docs are not indexed until `/wrapup`.

## Key concepts to know

- [[concepts/speaker-attribution]] — two-stage LLM pipeline; production recursive SI `si-gem31fl-recursive` (2026-07-01); Stage 2 hydration O(U log W + W) with FR-6 membership (2026-06-15)
- [[concepts/actionable-signal]] — the final output; a VIEW not a table; proof-segment speaker-gated
- [[concepts/proof-segment-speaker-resolution]] — evidence quote speakers → Finfluencer; display_name at sync (2026-07-01)
- [[concepts/signal-performance]] — how picks become measured horizons; truncation + implicit flip; SPY benchmark at `BBG000BDTBL9`; as-of exit pricing (2026-06-30)
- [[concepts/llm-config-registry]] — multi-model experimentation; production `si-gem31fl-recursive` / `fe-gem31fl-recursive`
- [[concepts/huf-eur-pipeline-pricing]] — rattaproff: HUF sheet vs per-site EUR recompute for Woo change detection
- [[concepts/resolution-pipeline-efficiency]] — JW matcher, re-attempt union, backlog sweeps for instrument/speaker resolution
- [[concepts/curation-learning]] — fund-noise similarity, unique-ticker bar 0.85, Stage 0.75 promotion (2026-07-07)
- [[concepts/linkedin-enrichment]] — trust-tiered LinkedIn URLs; trusted-only Supabase sync; no third-party API (2026-07-23)
- [[concepts/subscription-entitlement-ssot]] — profile tier = app entitlement; Stripe billing-only; reconcile + RLS close (2026-07-27)
- [[concepts/onboarding-new-podcast-source]] — gor_dagster: add a podcast RSS end-to-end (regex gate; SI auto-discovers `podcast_rss` since 2026-07-01)
- [[concepts/botastico-ssl-certificates]] — botastico: GCP LB managed certs, auto-renew, renewal-failure-only alerts (2026-07-15)

## Open threads

- Botastico: **`botastico-api`** + **`botastico`** monorepo indexed (SSL/monitoring 2026-07-15); portal/script/stripe/etc. still thin in wiki
- `rattaproff` — architecture note on HUF/EUR pricing synced (May 2026)
- **`gor_dagster` feature folders in flight** — wiki gains permanent `docs/architecture/features/` entries as each wraps up
- **finfluencer-tracker:** Terminal SKU / product map deferred; magic-link deliverability deferred
