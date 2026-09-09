---
type: product
title: "finfluencer.trade"
product: finfluencer-trade
project: null
created: 2026-04-06
updated: 2026-09-09
tags: [finfluencer, finance, pipeline, dagster, blog, tracking, linkedin, stripe, entitlement, charts, clarity, session-replay, feedback, lcp, ticker, lookup, onboarding]
---

# finfluencer.trade

Financial influencer tracking and accountability platform. Ingests podcast and content from financial influencers, transcribes it, extracts stock picks and recommendations, tracks prediction performance against market benchmarks, and publishes research and insights.

## What it does

The platform follows financial influencers (podcasters, YouTubers, analysts) and holds their predictions accountable. It processes audio content through a multi-stage pipeline — transcription → speaker attribution → facts extraction → signal generation — and tracks whether the picks actually played out. Findings are published via a public blog and directory.

## Target users

- Retail investors who want to assess finfluencer track records before following advice
- Researchers studying crowd-sourced financial prediction quality
- Andres as the operator building and refining the platform

## Component repos

| Repo | Role |
|---|---|
| [[projects/gor_dagster]] | Data pipeline — ingestion, transcription, speaker attribution, facts extraction, signal generation. The core backend. |
| [[projects/gor-blog]] | Public MkDocs site — blog, directory, articles; private **`research/cramer/`** for Cramer internal export scripts and specs (public data kit: [[projects/cramer-mad-money-research]]) |
| [[projects/finfluencer-tracker]] | App + landing on Vercel — auth, billing, leaderboards, **post-signup onboarding** (**2026-08-31** / wrap **2026-09-09** [[concepts/post-signup-onboarding]]); **finfluencer ticker lookup** (**2026-09-05** [[concepts/finfluencer-ticker-pick-lookup]]); cumulative charts / `/compare` / export (**2026-08-04** [[concepts/cumulative-performance-charts]]); marketing **Explore** nav to Leaderboard / Compare / Shows (**2026-08-17**); Reddit pixel `SignUp` (**2026-08-17** [[concepts/reddit-ads-conversion-tracking]]); **Clarity session replay** (**2026-08-18** [[concepts/session-replay-analytics]]); `/feedback` **Declined + admin note** (**2026-08-22** [[concepts/feedback-roadmap]]); **mobile CWV** lab LCP under 2.5 s (**2026-08-22** [[concepts/core-web-vitals-mobile]]); **field CWV + DATA_READY** (**2026-09-08** [[concepts/core-web-vitals-field]]); entitlement SSOT **2026-07-27** ([[concepts/subscription-entitlement-ssot]]) |
| [[projects/cramer-mad-money-research]] | Public reproducibility + working paper (SSRN 6643379) — Cramer / *Mad Money* 2018–2024 |

## Architecture summary

Three-tier cloud stack on GCP. Dagster Cloud for orchestration (hybrid deployment with GCE agent). BigQuery as the data warehouse. GCS for media files and transcripts. A multi-stage pipeline:

1. RSS ingestion → episode discovery
2. STT transcription (AssemblyAI, Deepgram, ElevenLabs) → raw transcripts
3. Unified transcript generation → normalised format
4. Speaker attribution (LLM) → named speaker labels
5. Facts extraction (LLM) → stock picks, position disclosures
6. Signal refinement → `ActionableSignal` records
7. Performance tracking → signal outcomes against market benchmarks

LLM layer uses multi-provider configuration registry (Gemini, GPT, Claude) with batch processing and consensus algorithms for accuracy.

## Current status

Active development. Pipeline is production-ready for core transcription and facts extraction. Speaker attribution and signal tracking are mature. Blog is live with 14+ published posts. Platform-update post + Kit send **2026-08-14** ([[projects/gor-blog]]). **Signal source quotes** restored (**2026-08-11** — [[concepts/signal-source-quote]]). **LinkedIn outreach** Notion drafts + chart videos shipped (**2026-08-11** — [[concepts/linkedin-outreach]]; human LinkedIn send only). **Chit Chat Stocks** onboarded (**2026-08-02**). **The Intrinsic Value Podcast** onboarded (**2026-08-22** — Pattern 7; 12th `podcast_rss` source). **Gemini 3.5 Flash-Lite** is production SI/FE (**2026-07-27**). **LinkedIn enrichment** wrapped **2026-07-23** ([[concepts/linkedin-enrichment]]). **App:** **post-signup onboarding** live (**2026-08-31**, wrapped **2026-09-09** — auth-callback detour, skippable — [[concepts/post-signup-onboarding]]); **finfluencer ticker lookup** live (**2026-09-05** — `finfluencer_ticker_list`; two box-plot charts retired — [[concepts/finfluencer-ticker-pick-lookup]]); **field CWV** wrapped (**2026-09-08** — field decides done; `/` teaser DATA_READY still ~4 s — [[concepts/core-web-vitals-field]]); **mobile CWV** delivery live (lab `/` LCP 2.03 s — [[concepts/core-web-vitals-mobile]]); `/feedback` **Declined + public admin note** (**2026-08-22** — [[concepts/feedback-roadmap]]); **Clarity session replay** (Production, consent-gated, masked, **2026-08-18** — [[concepts/session-replay-analytics]]); marketing **Explore** nav (Leaderboard / Compare / Shows, **2026-08-17**); **Reddit Ads pixel** (`SignUp` + `PageVisit`, **2026-08-17** — [[concepts/reddit-ads-conversion-tracking]]); **cumulative performance charts** (**2026-08-04**); **subscription entitlement SSOT** (**2026-07-27**); **CNBC IPO scoreboard** `/cnbc-ipo` (**2026-07-11**). **Google Ads PMax creatives** live (**2026-09-04** — V1 + V4; [[concepts/google-ads-creative-assets]]).

## Key cross-repo decisions

- **Field Core Web Vitals (2026-09-08):** field first-party p75 decides done; lab decides whether a change helped. `/` teaser DATA_READY still misses 2.0 s ([[concepts/core-web-vitals-field]], [[decisions/field-authoritative-cwv-2026-09]])
- **Mobile Core Web Vitals (2026-08-22):** client-rendered SPA stays; Search Console group is half MkDocs so a SPA-only fix cannot guarantee the *group* goes Good. Lab-as-gate **amended 2026-09-08** ([[concepts/core-web-vitals-mobile]])
- **Native feedback board (2026-06-02, declined 2026-08-22):** in-app `/feedback` with `notify_requested_at` intent marker; never rewrite `handle_feedback_notification()`; vote allowlist on both RLS policies ([[concepts/feedback-roadmap]])
- **Post-signup onboarding (2026-08-31 / wrap 2026-09-09):** one trigger at the auth callback; 24-hour route gate removed; funnel counts in GA4 not Clarity; skippable ([[concepts/post-signup-onboarding]])
- **Clarity session replay (2026-08-18):** Production-only Microsoft Clarity; same analytics-consent gate as GA4/Reddit; Balanced + Settings page-root mask; funnel events in GA4 (not Ads conversions); weekly review until **2026-09-17** ([[concepts/session-replay-analytics]])
- **Reddit pixel (2026-08-17):** consent-gated `pixel.js`; `SignUp` rides `maybeTrackSignUp`; Conversions campaign live, Traffic Max paused ([[concepts/reddit-ads-conversion-tracking]])
- **Cumulative charts / SPY sync (2026-07-30 → 2026-08-04):** App RPC builds chain-linked equity curves from mirrored signals + SPY `benchmark_daily_prices` synced from BigQuery `PriceHistory`; waypoint-shaped intra-window path (not true daily marks) ([[concepts/cumulative-performance-charts]])
- **LinkedIn URLs (2026-07-23):** Only trusted provenance syncs BQ → Supabase `finfluencers.linkedin_url` → tracker profiles; discovery never auto-publishes ([[concepts/linkedin-enrichment]])
- **LinkedIn outreach (2026-08-11):** Notion Accepted drafts + optional claim=chart MP4; no LinkedIn API send ([[concepts/linkedin-outreach]])
- **Google Ads PMax creatives (2026-09-04):** Manual monthly render from the shipped chart encoder; coverage ranking not alpha; write-once GCS; no Ads API ([[concepts/google-ads-creative-assets]])
- **Finfluencer ticker lookup (2026-09-05):** Unbounded `signals` aggregates go through a server-side RPC (`finfluencer_ticker_list`); definer re-gates `get_user_tier()`; pipeline-owned snapshots retire frontend → job → DROP ([[concepts/finfluencer-ticker-pick-lookup]])
- **Source quotes (2026-08-11):** `raw_source_quote` from proof_segments; blank beats approximate; synced to Supabase `signals` ([[concepts/signal-source-quote]])
- STT transcripts stored in `gor-stt-transcripts` GCS bucket (hardcoded, not from env var)
- **Backend data (BigQuery / GCS):** all pipeline and integration environments — local, branch, and production — use the **production** datasets and buckets (`dagster_prod`, `dagster_shared`, shared media/STT storage). There is no separate staging warehouse for backend analytics (see [[entities/bigquery]]).
- **finfluencer-tracker (app layer):** **two Supabase instances** — one for **production** and one for **development** — so app/auth data can be isolated while the app still reads pipeline data sourced from production backend stores.
- Facts extraction uses `ActionableSignal` VIEW over `PotentialPrediction` table — deduplicates by FE config priority (`fe-gem35fl-recursive` rank 1 since **2026-07-27**)
- Blog (`gor-blog`) uses MkDocs; its `docs/` folder is the site source, not project documentation
- **Public app funnel (2026-07-10, nav 2026-08-17):** leaderboards and `/upgrade` public; marketing header/footer Explore menu to Leaderboard / Compare / Shows; monetization at Spectator→Trader column mask + profile depth; Googlebot gets SPA not og-meta empty body ([[projects/finfluencer-tracker]])
- **Entitlement SSOT (2026-07-27):** `user_profiles.subscription_tier` is app entitlement; Stripe is billing-only; webhook + reconcile self-heal; combined-performance base SELECT closed ([[concepts/subscription-entitlement-ssot]])

## Planning and strategy (durable docs)

Use these when you need **growth**, **app MVP scope**, or **post-MVP product backlog** — not transient feature folders.

| What | Where |
|---|---|
| Growth / GTM / pre-launch plan | [`gor-blog/growth_plan.md`](file:///Users/andreskull/gor-blog/growth_plan.md) (repo root, not under `docs/`) |
| Shipped **finfluencer-tracker** MVP (achievement spec) | [`gor_dagster/docs/MVP_MASTER_PLAN.md`](file:///Users/andreskull/gor_dagster/docs/MVP_MASTER_PLAN.md) |
| Deferred product work after MVP | [`gor_dagster/docs/INCR_01_MASTER_PLAN.md`](file:///Users/andreskull/gor_dagster/docs/INCR_01_MASTER_PLAN.md) |
| App runbook (access, Stripe, E2E, blog URL) | [`gor_dagster/docs/operations/finfluencers-app-runbook.md`](file:///Users/andreskull/gor_dagster/docs/operations/finfluencers-app-runbook.md) |

**Not indexed here for planning:** `gor_dagster/docs/features/<feature>/` — temporary spec folders during active development; after `/wrapup`, durable write-ups land under `gor_dagster/docs/architecture/` or `docs/architecture/features/`. See [[projects/gor_dagster]].

## Related pages

- [[projects/gor_dagster]]
- [[projects/gor-blog]]
- [[projects/cramer-mad-money-research]]
- [[projects/finfluencer-tracker]]
- [[concepts/core-web-vitals-field]]
- [[concepts/core-web-vitals-mobile]]
- [[decisions/field-authoritative-cwv-2026-09]]
- [[concepts/feedback-roadmap]]
- [[concepts/session-replay-analytics]]
- [[concepts/post-signup-onboarding]]
- [[concepts/reddit-ads-conversion-tracking]]
- [[concepts/google-ads-conversion-tracking]]
- [[concepts/google-ads-creative-assets]]
- [[concepts/finfluencer-ticker-pick-lookup]]
- [[concepts/cumulative-performance-charts]]
- [[concepts/subscription-entitlement-ssot]]
- [[concepts/linkedin-enrichment]]
- [[concepts/linkedin-outreach]]
- [[concepts/signal-source-quote]]
- [[concepts/actionable-signal]]
- [[concepts/speaker-attribution]]
- [[entities/dagster]]
