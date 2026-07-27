---
type: product
title: "finfluencer.trade"
product: finfluencer-trade
project: null
created: 2026-04-06
updated: 2026-07-27
tags: [finfluencer, finance, pipeline, dagster, blog, tracking, linkedin, stripe, entitlement]
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
| [[projects/finfluencer-tracker]] | App + landing on Vercel — auth, billing, leaderboards, conversion funnel; entitlement SSOT **2026-07-27** ([[concepts/subscription-entitlement-ssot]]) |
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

Active development. Pipeline is production-ready for core transcription and facts extraction. Speaker attribution and signal tracking are mature. Blog is live with 14+ published posts. **Gemini 3.5 Flash-Lite** is production SI/FE (**2026-07-27** — `si-gem35fl-recursive` @450s / `fe-gem35fl-recursive` @1800s; ActionableSignal FE priority 1). **LinkedIn enrichment** wrapped **2026-07-23** ([[concepts/linkedin-enrichment]]). **App (finfluencer-tracker):** **subscription entitlement SSOT** live on production (**2026-07-27** — profile tier for UI + RLS; Stripe billing-only; [[concepts/subscription-entitlement-ssot]]). **CNBC IPO scoreboard** at **`/cnbc-ipo`** (**2026-07-11**). Public conversion funnel (**2026-07-10** — see [[projects/finfluencer-tracker]]).

## Key cross-repo decisions

- **LinkedIn URLs (2026-07-23):** Only trusted provenance syncs BQ → Supabase `finfluencers.linkedin_url` → tracker profiles; discovery never auto-publishes ([[concepts/linkedin-enrichment]])
- STT transcripts stored in `gor-stt-transcripts` GCS bucket (hardcoded, not from env var)
- **Backend data (BigQuery / GCS):** all pipeline and integration environments — local, branch, and production — use the **production** datasets and buckets (`dagster_prod`, `dagster_shared`, shared media/STT storage). There is no separate staging warehouse for backend analytics (see [[entities/bigquery]]).
- **finfluencer-tracker (app layer):** **two Supabase instances** — one for **production** and one for **development** — so app/auth data can be isolated while the app still reads pipeline data sourced from production backend stores.
- Facts extraction uses `ActionableSignal` VIEW over `PotentialPrediction` table — deduplicates by FE config priority (`fe-gem35fl-recursive` rank 1 since **2026-07-27**)
- Blog (`gor-blog`) uses MkDocs; its `docs/` folder is the site source, not project documentation
- **Public app funnel (2026-07-10):** leaderboards and `/upgrade` public; monetization at Spectator→Trader column mask + profile depth; Googlebot gets SPA not og-meta empty body ([[projects/finfluencer-tracker]])
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
- [[concepts/subscription-entitlement-ssot]]
- [[concepts/linkedin-enrichment]]
- [[concepts/actionable-signal]]
- [[concepts/speaker-attribution]]
- [[entities/dagster]]
