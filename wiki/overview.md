---
type: overview
title: "Wiki Overview"
created: 2026-04-06
updated: 2026-09-04
---

# Wiki Overview

Three products, one methodology, one wiki. Updated: 2026-09-04.

## Products

**[[products/finfluencer-trade]]** — Financial influencer accountability platform. Ingests podcast content, transcribes it, extracts stock picks, tracks prediction performance. Core repos: `gor_dagster` (data pipeline), `gor-blog` (public site + **`api/newsletter/`**; private **`research/cramer/`** — platform-update post + Kit send **2026-08-14**), **`finfluencer-tracker`** (Vercel app + landing — **Google Ads PMax creatives** **2026-09-04** [[concepts/google-ads-creative-assets]]; **mobile CWV** **2026-08-22** [[concepts/core-web-vitals-mobile]]; **GA4 instrumentation gap fixed + BigQuery export** **2026-09-02** [[decisions/ga4-instrumentation-registration-2026-09]]; **`/feedback` Declined + admin note** **2026-08-22** [[concepts/feedback-roadmap]]; **Clarity session replay** **2026-08-18** [[concepts/session-replay-analytics]]; marketing **Explore** nav **2026-08-17**; **Reddit Ads pixel** **2026-08-17** [[concepts/reddit-ads-conversion-tracking]]; **cumulative performance charts** **2026-08-04** [[concepts/cumulative-performance-charts]]; **subscription entitlement SSOT** **2026-07-27** [[concepts/subscription-entitlement-ssot]]; **CNBC IPO scoreboard** `/cnbc-ipo` **2026-07-11**; public conversion funnel **2026-07-10**; see [[projects/finfluencer-tracker]]). **`cramer-mad-money-research`** — public kit + SSRN [6643379](https://ssrn.com/abstract=6643379). Pipeline production-ready; **Gemini 3.5 Flash-Lite** production SI/FE (**2026-07-27**); **signal source-quote restoration** + **LinkedIn outreach** (Notion drafts + chart videos; manual LinkedIn send) wrapped **2026-08-11** ([[concepts/signal-source-quote]], [[concepts/linkedin-outreach]]); **LinkedIn enrichment** **2026-07-23** ([[concepts/linkedin-enrichment]]). Active `gor_dagster` feature folders under `docs/features/` (batch-integration, post-cutoff IPO, others).

**[[products/rattaproff]]** — WooCommerce multi-store automation for a network of **20** e-commerce storefronts. Single repo. Handles supplier ingestion, **HUF→EUR pricing** (default 350 HUF/EUR; per-site overrides optional — [[concepts/huf-eur-pipeline-pricing]]), publishing, and Google Indexing API quota management. **GSheet ground-truth sync safety** (July 2026) — grow-only robot writes, `__sync_status` marker, GCS fallback — [[concepts/gsheet-ground-truth-sync]]. **Category URL export** (2026-08-14) — Woo REST slugs → gitignored `category_urls_*.csv`. Repo docs: `docs/architecture/huf-eur-per-site-pricing.md`, `docs/architecture/features/gsheet-ground-truth-sync.md`, `docs/architecture/category-url-export.md`.

**[[products/botastico]]** — [[projects/botastico-api]] (2026-05-18): Cloud Run API, chat image attachments, Pub/Sub → `slack_chat_logs`. **[[projects/botastico]]** (2026-07-15): GCP LB SSL for `chatapps`/`assets` widget delivery; July 2026 cert incident fixed; renewal-failure monitoring — [[concepts/botastico-ssl-certificates]]. Other botastico repos still light in wiki.

## Methodology

**[[projects/spec-driven-ai-coding]]** — The development process used across all projects. Spec → Plan → Execute → Wrapup. Provides global `/` commands for Cursor and Antigravity. The `/wrapup` command is the bridge between completed features and the wiki.

## Active development

`gor_dagster` has in-progress feature folders under `docs/features/` (batch-integration, post-cutoff IPO resolution, etc.). **Google Ads PMax creatives** wrapped **2026-09-04** (V1 + V4 live). **The Intrinsic Value Podcast** RSS onboarding wrapped **2026-08-22**. **finfluencer-tracker mobile Core Web Vitals** wrapped **2026-08-22**. **finfluencer-tracker feedback decline with admin note** wrapped **2026-08-22**. **finfluencer-tracker session replay (Clarity)** wrapped **2026-08-18**. **Signal source-quote restoration** + **LinkedIn outreach** wrapped **2026-08-11**. **finfluencer-tracker marketing Explore nav** wrapped **2026-08-17**. **finfluencer-tracker Reddit pixel** wrapped **2026-08-17**. **finfluencer-tracker cumulative performance charts** wrapped **2026-08-04**. **gor-blog platform-update blog + Kit send** wrapped **2026-08-14**. Temporary feature docs are not indexed until `/wrapup`.

## Key concepts to know

- [[concepts/speaker-attribution]] — two-stage LLM pipeline; production recursive SI `si-gem35fl-recursive` @450s (2026-07-27); Stage 2 hydration O(U log W + W) with FR-6 membership (2026-06-15)
- [[concepts/actionable-signal]] — the final output; a VIEW not a table; proof-segment speaker-gated; FE priority 1 = `fe-gem35fl-recursive` (2026-07-27)
- [[concepts/proof-segment-speaker-resolution]] — evidence quote speakers → Finfluencer; display_name at sync (2026-07-01)
- [[concepts/signal-performance]] — how picks become measured horizons; truncation + implicit flip; SPY benchmark at `BBG000BDTBL9`; as-of exit pricing (2026-06-30)
- [[concepts/llm-config-registry]] — multi-model experimentation; production `si-gem35fl-recursive` / `fe-gem35fl-recursive` (2026-07-27)
- [[concepts/huf-eur-pipeline-pricing]] — rattaproff: HUF sheet vs per-site EUR recompute for Woo change detection
- [[concepts/resolution-pipeline-efficiency]] — JW matcher, re-attempt union, backlog sweeps for instrument/speaker resolution
- [[concepts/curation-learning]] — fund-noise similarity, unique-ticker bar 0.85, Stage 0.75 promotion (2026-07-07)
- [[concepts/linkedin-enrichment]] — trust-tiered LinkedIn URLs; trusted-only Supabase sync; no third-party API (2026-07-23)
- [[concepts/signal-source-quote]] — `raw_source_quote` from proof_segments; blank beats approximate; backfill + FE forward fix (2026-08-11)
- [[concepts/linkedin-outreach]] — Notion Accepted drafts + optional chart MP4; human LinkedIn send only (2026-08-11)
- [[concepts/google-ads-creative-assets]] — monthly PMax images + video from the shipped chart encoder; coverage ranking; write-once GCS (2026-09-04)
- [[concepts/subscription-entitlement-ssot]] — profile tier = app entitlement; Stripe billing-only; reconcile + RLS close (2026-07-27)
- [[concepts/cumulative-performance-charts]] — profile `/compare` cumulative % curves + export; waypoint-shaped path; SPY `benchmark_daily_prices` (2026-08-04)
- [[concepts/reddit-ads-conversion-tracking]] — Reddit pixel `SignUp` / `PageVisit`; consent gate before `pixel.js`; Conversions campaign live (2026-08-17)
- [[concepts/session-replay-analytics]] — Microsoft Clarity on Production; analytics consent; Balanced + Settings mask; weekly review until 2026-09-17 (2026-08-18)
- [[concepts/feedback-roadmap]] — native `/feedback` board; Declined + public note; `notify_requested_at` intent marker; never rewrite `handle_feedback_notification()` (2026-08-22)
- [[concepts/core-web-vitals-mobile]] — SPA delivery: lab LCP under 2.5 s on `/`; Search Console group half MkDocs; field data ~2026-09-19 (2026-08-22)
- [[concepts/onboarding-new-podcast-source]] — gor_dagster: add a podcast RSS end-to-end (regex gate; SI auto-discovers `podcast_rss`; Finfluencers Directory required since 2026-07-27)
- [[concepts/botastico-ssl-certificates]] — botastico: GCP LB managed certs, auto-renew, renewal-failure-only alerts (2026-07-15)

## Open threads

- Botastico: **`botastico-api`** + **`botastico`** monorepo indexed (SSL/monitoring 2026-07-15); portal/script/stripe/etc. still thin in wiki
- `rattaproff` — category URL export from Woo slugs (2026-08-14); HUF/EUR pricing note (May 2026)
- **`gor_dagster` feature folders in flight** — wiki gains permanent `docs/architecture/features/` entries as each wraps up
- **finfluencer-tracker:** `T1.1a` (GA4 + Clarity field read of the CWV fix) earliest **~2026-09-05/06** — segmentable-data clock restarted 2026-09-02 after finding GA4 custom dimensions were never registered ([[decisions/ga4-instrumentation-registration-2026-09]]); Search Console CWV field data ~**2026-09-19**; Clarity weekly review until **2026-09-17**; official Clarity↔GA4 OAuth link deferred; Reddit CAPI / Advanced Matching deferred; Terminal SKU / product map deferred; magic-link deliverability deferred; true daily portfolio marks parked ([[concepts/core-web-vitals-mobile]], [[concepts/session-replay-analytics]], [[concepts/cumulative-performance-charts]], [[concepts/reddit-ads-conversion-tracking]])
