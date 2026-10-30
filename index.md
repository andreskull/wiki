---
type: index
title: "Wiki Index"
created: 2026-04-06
updated: 2026-10-30
---

# Wiki Index

Master catalog of all pages in this wiki. Update this file whenever a page is added, removed, or significantly renamed.

---

## Overview

- [[wiki/overview]] — Three products, one methodology, one wiki (Updated 2026-10-30)

---

## Products

| Page | Description |
|------|-------------|
| [[wiki/products/finfluencer-trade]] | Financial influencer accountability platform — planning hub; GTM living page [[wiki/synthesis/gtm-growth]] (Updated 2026-10-30) |
| [[wiki/products/rattaproff]] | WooCommerce multi-store automation — 20 storefronts; HUF→EUR pricing [[wiki/concepts/huf-eur-pipeline-pricing]]; gsheet sync safety [[wiki/concepts/gsheet-ground-truth-sync]]; category URL export (Woo slugs, 2026-08-14) |
| [[wiki/products/botastico]] | Partially indexed — [[projects/botastico]] SSL/monitoring (2026-07-15); [[projects/botastico-api]] (2026-05-18); other repos pending |

---

## Projects

| Page | Product | Status |
|------|---------|--------|
| [[wiki/projects/gor_dagster]] | finfluencer.trade | Active — **Prediction scores pipeline, model registry & dashboard** wrapped **2026-10-30** ([[concepts/prediction-scores]]; `blend_w50_v1` 6M outperformance scorer, per-cutoff `status = 'active'` registry `[T_k, T_{k+1})`, `score_recent_signals` + `mat_pick_scores` sync, evidence-backed `"Why N stars?"` explanations, monthly validation gate, automatic cutoff drift retraining `>= 500` rows & `>= 5%`, `/scoring-models` dashboard with Candidate Banner); **In the Money with Amber Kanwar** onboarded **2026-10-28** (15th `podcast_rss`; Transistor Pattern 9; 180 episodes); **Episode-title speaker hint** wrapped **2026-10-25** ([[concepts/episode-title-speaker-hint]]; authoritative publisher spellings guide recursive SI bootstrap canonicalization, `decide_title_guard` prevents auto-creating misspelled finfluencers, curation dashboard notes, 23 historical finfluencers corrected via reviewed manifest); **Foreign listing resolution** wrapped **2026-10-25** ([[concepts/foreign-listing-resolution]]; native TSX `.TO` & TSXV `.V` resolution, `choose_listing` policy, contained-name rule, 465 rows & 185 aliases repointed, 104 vacated FIGI-less OTC rows deleted); **Claude Haiku 5.5 evaluation & Option 3 Hybrid Routing** wrapped **2026-10-24** (promoted `si-haiku55-recursive` @450s to Rank #1 for SI, retained `fe-gem35fl-recursive` @1800s for FE, 4-tier fallback, memory fold bounds hardening); **Near-duplicate finfluencers** wrapped **2026-10-22** (`NEAR_NAME` cards, `duplicate_pair_match` + SQL mirror, `near_name_spelling` merge options, `FinfluencerPairDismissal` migration 027, create-dialog warning, batched proof-segment `MERGE`); **Ads review** wrapped **2026-10-20** (shared Claude/Grok files; Grok 08:10 routine; ank applies approved ad changes); **Market Call** wrapped **2026-10-14** (14th `podcast_rss`; Pattern 8; directory Covered `market-call`); **Retiring LLM models** wrapped **2026-10-07** (closed Sonnet 4.5 / GPT-5.1 eval configs; refuse not remap); **Sector ETF registry** wrapped **2026-10-01** ([[concepts/sector-etf-registry]]); **Information Ratio** wrapped **2026-09-29** ([[concepts/information-ratio]]); **post-cutoff IPO resolution** wrapped **2026-09-28** ([[concepts/post-cutoff-ipo-resolution]]); **social share previews** wrapped **2026-09-28**; **finfluencer, show & ticker profile pages** wrapped **2026-09-28** (FR-1.11 top/bottom decile capture cancelled before build); **pipeline dashboard users screen** wrapped **2026-09-24** (test users in `internal_accounts` since 2026-09-25; onboarding columns); **Hedge Fund Tips with Tom Hayes** wrapped **2026-09-22** (359/359 downloaded; directory deferred); **Google Ads PMax creatives** wrapped **2026-09-04** (V1 + V4 live); **The Intrinsic Value Podcast** **2026-08-22**; **source quotes** + **LinkedIn outreach** **2026-08-11**; Gemini 3.5 Flash-Lite SI/FE **2026-07-27** |
| [[wiki/projects/gor-blog]] | finfluencer.trade | Active — F5Bot mailbox scoring live **2026-10-13** (Olli, `_internal/f5bot/`); **Cramer v1 closed** **2026-09-24**; platform-update post + Kit send **2026-08-14**; CTA pattern; apex `/api/subscribe` |
| [[wiki/projects/finfluencer-tracker]] | finfluencer.trade | Active — **Prediction Scores (`1★–5★`) & Option 2 3-tier gating** wrapped **2026-10-30** ([[concepts/prediction-scores]]; 10-session fresh window `get_fresh_scored_picks` on `/signals` & profile `<FreshScoredPicksSection />`; Guest sees 3 featured finfluencers on aggregate views with sort-order leak closed before `ORDER BY`/`ROW_NUMBER()` + public `/signals` with scores masked; Spectator unlocks 100% of aggregate track records across all 84 finfluencers & 14 shows + `1★–5★` scores for 3 featured finfluencers; Trader unlocks all fresh `1★–5★` scored picks across all 84 finfluencers); **landing tour** wrapped **2026-10-20** (50/50 on `/`; first read 6/103 vs 7/112; split stays; Thursday review); **RUM off GA4** **2026-10-20**; **mobile experience uplift** **2026-10-20**; **conversion measurement** wrapped **2026-10-20** (`sign_up` €20 is the only primary; other key events stay secondary); **signed-out QA findings and data integrity** wrapped **2026-10-19** (11-area sweep, 38 tasks: snapshot 31-day gate and Monday cron, SPY price repair, single-paid-panelist 80% masking, clean 404s, default `/compare` prefilled, Q4 pick count predicate with is_kept in sector averages, mobile signup uplift); **mobile sign-up drop** closed **2026-10-16** (Search mix, not the deploy; non-US cut and Search Partners; `paywall_view` / `paywall_cta_click` live); **Search QS landing page** wrapped **2026-10-14** (heading plus supporting line; lost IS rank 72.42% vs 86.82%; page copy stays); **ad landing time to content** wrapped **2026-10-09** (shell starts the guest request; server HTML stayed closed); **onboarding skip improvements** **2026-10-06** (completion 6/14; Skip stays; loading masks are not hit targets); **Supabase ledger baseline** **2026-09-30** (migrations replay from empty; histories repaired); **Information Ratio** on leaderboard and profiles **2026-09-29** ([[concepts/information-ratio]]); **profile script split** **2026-09-28** (show data-ready; stand-ins on `development`; body split not shipped); **onboarding questionnaire v2** **2026-09-28**; **feedback e2e on demand** **2026-09-28**; **investor positioning** **2026-09-27**; **Search RSA stills** **2026-09-27**; **performance palette** **2026-09-25**; **sector name filter at every width** **2026-09-24**; **user-default holding period** **2026-09-16**; **post-signup onboarding** **2026-09-09**; **field CWV + DATA_READY** **2026-09-08**; **finfluencer ticker lookup** **2026-09-05**; **Google Ads PMax creatives** **2026-09-04**; **GA4 instrumentation gap fixed + BigQuery export** **2026-09-02**; **mobile CWV** **2026-08-22**; **feedback Declined + admin note** **2026-08-22**; **Clarity session replay** **2026-08-18**; **Explore nav** **2026-08-17**; **Reddit Ads pixel** **2026-08-17**; **cumulative performance charts** **2026-08-04**; **subscription entitlement SSOT** **2026-07-27** |
| [[wiki/projects/rattaproff]] | rattaproff | Operational — category URL export from Woo slugs **2026-08-14**; gsheet sync safety **2026-07-13**; permalink backfill + gsheet restore (Jun 2026); 20 storefronts |
| [[wiki/projects/spec-driven-ai-coding]] | (methodology) | Active |
| [[wiki/projects/cramer-mad-money-research]] | finfluencer.trade | v1 frozen **2026-09-24** — public kit + SSRN 6643379; do not overwrite `data/` |
| [[wiki/projects/botastico-api]] | botastico | Active — chat image attachments **2026-05-18** ([feature doc](file:///Users/andreskull/botastico-api/docs/architecture/features/botastico-chat-image-attachments.md)); Cloud Run Flask; `slack_chat_logs` Pub/Sub consumer |
| [[wiki/projects/botastico]] | botastico | Active — GCP LB SSL certs + monitoring **2026-07-15** (July widget outage); `chatapps`/`assets` auto-renew; [[concepts/botastico-ssl-certificates]] |

---

## Sources

| Page | Description |
|------|-------------|
| [[wiki/sources/2026-04-07-cramer-mad-money-performance-methodology]] | Cramer / Mad Money performance & methodology ([[projects/cramer-mad-money-research]]) |

---

## Concepts

| Page | Description |
|------|-------------|
| [[wiki/concepts/prediction-scores]] | gor_dagster + tracker: ex-ante `1★–5★` 6M outperformance scorer (`blend_w50_v1`), per-cutoff `[T_k, T_{k+1})` model registry, monthly validation gate, automatic cutoff drift retraining (`>= 500` rows & `>= 5%`), `/scoring-models` dashboard, 10-session fresh window (`get_fresh_scored_picks`), and Option 2 3-tier gating (`Guest` → `Spectator` → `Trader`) (2026-10-30) |
| [[wiki/concepts/speaker-attribution]] | Two-stage LLM pipeline; production recursive SI (`si-haiku55-recursive` @450s, 2026-10-24); Stage 2 hydration O(U log W + W), FR-6 membership; ElevenLabs mono-speaker hardening |
| [[wiki/concepts/actionable-signal]] | Final output VIEW — speaker-gated; FE priority 1 = `fe-gem35fl-recursive` (2026-07-27; paired with Haiku 5.5 SI 2026-10-24) |
| [[wiki/concepts/signal-performance]] | Signal → horizons: truncation, implicit flip; SPY benchmark `BBG000BDTBL9`; as-of exit pricing (2026-06-30) |
| [[wiki/concepts/llm-config-registry]] | Multi-model experimentation; Option 3 Hybrid Routing production `si-haiku55-recursive` / `fe-gem35fl-recursive` (2026-10-24) |
| [[wiki/concepts/spec-driven-development]] | The development methodology loop |
| [[wiki/concepts/huf-eur-pipeline-pricing]] | rattaproff: HUF sheet vs per-site EUR recompute for Woo diffs |
| [[wiki/concepts/gsheet-ground-truth-sync]] | rattaproff: grow-only sheet writes, `__sync_status` marker, GCS fallback (2026-07-13) |
| [[wiki/concepts/onboarding-new-podcast-source]] | gor_dagster: playbook for new RSS → ActionableSignal; directory PR required (Req 12); Pattern 8 Omny clip UUID + job-only `rel="next"` (2026-10-14); Pattern 7 PPLLC + slug freeze (2026-08-22); no idempotency requirement (retired 2026-08-19); iTunes feed discovery; SI auto-discovers podcast_rss |
| [[wiki/concepts/proof-segment-speaker-resolution]] | Proof-segment quote speakers → Finfluencer; ActionableSignal gate; display_name at sync (2026-07-01) |
| [[wiki/concepts/resolution-pipeline-efficiency]] | gor_dagster: JW matcher, re-attempt union, backlog sweeps, alias-on-resolve |
| [[wiki/concepts/curation-learning]] | gor_dagster: fund-noise similarity, unique-ticker bar 0.85, Stage 0.75 promotion (2026-07-07) |
| [[wiki/concepts/episode-title-speaker-hint]] | gor_dagster: authoritative episode-title guest spellings guide recursive SI bootstrap canonicalization, `decide_title_guard` auto-create guard, curation dashboard notes, 23 historical finfluencers corrected via reviewed manifest (2026-10-25) |
| [[wiki/concepts/foreign-listing-resolution]] | gor_dagster: native TSX (`.TO`) and TSXV (`.V`) matching, `choose_listing` policy, contained-name rule, 465 rows & 185 aliases repointed, 104 vacated FIGI-less OTC rows deleted (2026-10-25) |
| [[wiki/concepts/post-cutoff-ipo-resolution]] | gor_dagster: optional ticker, first EOD bar gate, `v_ticker_tenancy` for reused symbols (2026-09-28) |
| [[wiki/concepts/information-ratio]] | gor_dagster + tracker: kept-pick alpha / sample stddev, NULL below 20; replaced Sharpe (2026-09-29) |
| [[wiki/concepts/sector-etf-registry]] | gor_dagster: sector calls → concept ETF (GICS = benchmark ETFs); dismissed → registry → alias → human; Map to sector concept (2026-10-01) |
| [[wiki/concepts/linkedin-enrichment]] | gor_dagster + tracker: trust-tiered LinkedIn URLs; trusted-only Supabase sync; no third-party API (2026-07-23) |
| [[wiki/concepts/signal-source-quote]] | gor_dagster: `raw_source_quote` from proof_segments; blank beats approximate; backfill + FE forward fix (2026-08-11) |
| [[wiki/concepts/linkedin-outreach]] | gor_dagster + tracker: Notion Accepted drafts + optional chart MP4; human LinkedIn send only (2026-08-11) |
| [[wiki/concepts/google-ads-creative-assets]] | gor_dagster + tracker: monthly PMax images + video from the shipped chart encoder; ads subject period is 6 months (2026-09-27); Search RSA studio stills separate; coverage ranking; write-once GCS (2026-09-04) |
| [[wiki/concepts/finfluencer-ticker-pick-lookup]] | finfluencer-tracker: profile ticker lookup + ranked lists share `finfluencer_ticker_list`; PostgREST 1000-row cap; two box-plot charts retired (2026-09-05) |
| [[wiki/concepts/subscription-entitlement-ssot]] | finfluencer-tracker: profile tier = app entitlement; Option 2 3-tier gating (`Guest` → `Spectator` → `Trader`, 2026-10-30); Stripe billing-only; reconcile + RLS close (2026-07-27; updated 2026-10-30) |
| [[wiki/concepts/cumulative-performance-charts]] | finfluencer-tracker: cumulative % profile + `/compare` + watermarked export; SPY sync; waypoint-shaped path (2026-08-04) |
| [[wiki/concepts/botastico-ssl-certificates]] | botastico: GCP LB managed certs, auto-renew, renewal-failure monitoring (2026-07-15) |
| [[wiki/concepts/google-ads-conversion-tracking]] | finfluencers.trade paid measurement — ONE Google tag `G-BLE3H05Q5T` → GA4 `485294334` + `AW-18322362149`; `sign_up` is the only primary (wrapped 2026-10-20); trust IDs not names |
| [[wiki/concepts/reddit-ads-conversion-tracking]] | finfluencers.trade Reddit pixel and Conversions API — `SignUp` + `PageVisit`, consent gate before `pixel.js`; server SignUp live; €10/day test budget kept (2026-10-09) |
| [[wiki/concepts/session-replay-analytics]] | finfluencers.trade Microsoft Clarity — Production-only replay; analytics consent; Balanced + Settings mask; funnel events never Ads conversions (2026-08-18) |
| [[wiki/concepts/post-signup-onboarding]] | finfluencer-tracker: auth-callback onboarding detour; one research question per screen (2026-09-28); skippable; skip-path work measured 2026-10-06 (completion 43%, Skip stays); funnel counts in GA4 not Clarity (2026-09-09) |
| [[wiki/concepts/user-default-holding-period]] | finfluencer-tracker: system start 6 months; Settings pin; named `?horizon=` wins; URL omit uses system default (2026-09-16) |
| [[wiki/concepts/feedback-roadmap]] | finfluencer-tracker native `/feedback` board — Declined + public admin note; `notify_requested_at` intent marker; vote allowlist; never rewrite `handle_feedback_notification()`; board smoke is on demand (`npm run test:e2e:feedback`) because fixture inserts spend Resend credits (2026-08-22; e2e opt-in 2026-09-28) |
| [[wiki/concepts/core-web-vitals-field]] | finfluencer-tracker first-party field RUM + DATA_READY; field decides done; show surface added 2026-09-28; `/` teaser still ~4 s (2026-09-08); sink moved from GA4 to `/api/rum` + Supabase 2026-10-12 |
| [[wiki/concepts/core-web-vitals-mobile]] | finfluencer-tracker SPA delivery — lab LCP under 2.5 s on `/`; Supabase realtime/storage stand-ins on `development` (2026-09-28); Search Console group half MkDocs (2026-08-22; lab-as-gate amended 2026-09-08) |

---

## Entities

| Page | Description |
|------|-------------|
| [[wiki/entities/bigquery]] | Google Cloud data warehouse — used across products |
| [[wiki/entities/dagster]] | Data orchestration — backbone of gor_dagster |
| [[wiki/entities/gcs]] | Object storage — media and STT transcripts (gor_dagster) |

---

## Decisions

| Page | Description |
|------|-------------|
| [[wiki/decisions/claude-haiku-55-hybrid-routing]] | gor_dagster: promote Claude Haiku 5.5 for SI (`si-haiku55-recursive` @450s); retain Gemini 3.5 Flash-Lite for FE (`fe-gem35fl-recursive` @1800s); Option 3 Hybrid Routing (2026-10-24) |
| [[wiki/decisions/botastico-gcp-managed-ssl-2026-07]] | botastico: stay on GCP managed LB certs; alert on renewal failure only (2026-07-15) |
| [[wiki/decisions/exclude-regulated-finance-employees-from-outreach-2026-08]] | finfluencer.trade: qualify outreach by employer; hold contacts at registered entities from automated performance messaging (2026-08-31) |
| [[wiki/decisions/ga4-instrumentation-registration-2026-09]] | finfluencer-tracker (pre-2026-10-12 RUM windows only): register 10 GA4 custom dimensions + `metric_value` metric, link BigQuery export to `gurus-on-record` (US multi-region, kept) after finding registration was never done (2026-09-02) |
| [[wiki/decisions/field-authoritative-cwv-2026-09]] | finfluencer-tracker: field first-party p75 decides CWV done; lab decides whether a change helped (2026-09-08) |

---

## Synthesis

| Page | Description |
|------|-------------|
| [[wiki/synthesis/gtm-growth]] | Living GTM / growth hub for finfluencers.trade — live channels, open priorities, doc map. F5Bot rebuild closed 2026-10-13. |
| [[wiki/synthesis/lint-finfluencer-trade-2026-04-06]] | Wiki lint — finfluencer.trade product scope (2026-04-06) |
| [[wiki/synthesis/lint-gor-blog-2026-04-06]] | Wiki lint — gor-blog project (2026-04-06) |
| [[wiki/synthesis/lint-finfluencer-tracker-2026-04-06]] | Wiki lint — finfluencer-tracker project (2026-04-06) |
| [[wiki/synthesis/lint-gor-blog-internal-papers-2026-04-07]] | Wiki lint — gor-blog `research/` papers ingested to vault (2026-04-07) |

---

## Meta

| Page | Description |
|------|-------------|
| [[USER_GUIDE.md]] | Human-facing guide — how to use the wiki |
| [[CLAUDE.md]] | Operating manual for Claude, Cursor, Antigravity |
| [[log.md]] | Append-only change log |
| [[index.md]] | This file |
| [[requirements.md]] | Original design document for this wiki |
