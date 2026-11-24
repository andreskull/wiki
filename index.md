---
type: index
title: "Wiki Index"
created: 2026-04-06
updated: 2026-11-24
---

# Wiki Index

Master catalog of all pages in this wiki. Update this file whenever a page is added, removed, or significantly renamed.

---

## Overview

- [[wiki/overview]] — Three products, one methodology, one wiki (Updated 2026-11-24)

---

## Products

| Page | Description |
|------|-------------|
| [[wiki/products/finfluencer-trade]] | Financial influencer accountability platform — planning hub; GTM living page [[wiki/synthesis/gtm-growth]] (Updated 2026-11-24) |
| [[wiki/products/rattaproff]] | WooCommerce multi-store automation — 20 storefronts; 65-col slim GSheet [[wiki/concepts/gsheet-ground-truth-sync]] (2026-11-21); normalized Woo comparison, SQL cache & draft indexing cutting runs to 7.4h (2026-11-21); HUF→EUR pricing [[wiki/concepts/huf-eur-pipeline-pricing]] |
| [[wiki/products/botastico]] | Partially indexed — [[projects/botastico-portal]] (2026-11-19); [[projects/botastico-api]] (2026-11-19); [[projects/botastico]] SSL/monitoring (2026-07-15); other repos pending |

---

## Projects

| Page | Product | Status |
|------|---------|--------|
| [[wiki/projects/gor_dagster]] | finfluencer.trade | Active — **Show entry timing** wrapped **2026-11-21** (`next_market_open` across all 18 shows; `111,383` full / `71,801` pruned recomputed performance rows synced to dev `base_version = 12` & prod `base_version = 21`; DML timing seeder, `--reason` repair, snapshot cleaner & parity verifier; Joe Terranova prod `backtest_compute` `+235.1%` → `+192.52%` vs S&P 500 `+209.31%`); **Backtest price fixes** wrapped **2026-11-21** (`build_mat_instrument_daily_prices_sql` US-session projection, `SICPQ`/`HRBR` OTC spike fixes cutting *The Compound and Friends* max drawdown `-79.7%` → `-28.7%` on prod `base_version = 17`, `10x` spike guard `SKIPPED_SPIKE_GUARD`, snapshot cleaner & verifier); **Scribe v3** wrapped **2026-11-14** ([[decisions/scribe-v3-default]]; new transcriptions use `elevenlabs_scribe_v3`, v1 and v2 stay valid); **Instrument identity and price integrity** wrapped **2026-11-13** (`v_ticker_tenancy`, isolated price sync, `vix_at_signal`, `choose_listing`, pre-sync guards); **Factory `AGENTS.md` switch + 10 domain/ops reference docs** **2026-11-12** (`staging`, 3-way BigQuery DDL rule, safe `pytest` track); **Schwab Network** wrapped **2026-11-12** (17th `podcast_rss`, `content_source_id` `2692382b-dabf-452a-a69f-60196e00ff5e`; Pattern 5; proof gate 17/17; live feed 9,975/9,975); **Market Hang** wrapped **2026-11-12** (16th `podcast_rss`; Pattern 1; 37/37 downloaded); **Share-class map** wrapped **2026-11-10** ([[concepts/share-class-identity]]; `utils/share_classes.py`, incremental sync of 14 rows); **Prediction scores pipeline, model registry & dashboard** wrapped **2026-10-30** ([[concepts/prediction-scores]]; `blend_w50_v1` 6M outperformance scorer, per-cutoff `status = 'active'` registry `[T_k, T_{k+1})`, `score_recent_signals` + `mat_pick_scores` sync, evidence-backed `"Why N stars?"` explanations, monthly validation gate, automatic cutoff drift retraining `>= 500` rows & `>= 5%`, `/scoring-models` dashboard with Candidate Banner); **In the Money with Amber Kanwar** wrapped **2026-11-05** (15th `podcast_rss`, `content_source_id` `5a768ce1-3e15-4029-b66a-e7d520c6018f`; Transistor Pattern 9; 175 downloaded of 182); **Episode-title speaker hint** wrapped **2026-10-25** ([[concepts/episode-title-speaker-hint]]; authoritative publisher spellings guide recursive SI bootstrap canonicalization, `decide_title_guard` prevents auto-creating misspelled finfluencers, curation dashboard notes, 23 historical finfluencers corrected via reviewed manifest); **Foreign listing resolution** wrapped **2026-10-25** ([[concepts/foreign-listing-resolution]]; native TSX `.TO` & TSXV `.V` resolution, `choose_listing` policy, contained-name rule, 465 rows & 185 aliases repointed, 104 vacated FIGI-less OTC rows deleted); **Claude Haiku 5.5 evaluation & Option 3 Hybrid Routing** wrapped **2026-10-24** (promoted `si-haiku55-recursive` @450s to Rank #1 for SI, retained `fe-gem35fl-recursive` @1800s for FE, 4-tier fallback, memory fold bounds hardening); **Near-duplicate finfluencers** wrapped **2026-10-22** (`NEAR_NAME` cards, `duplicate_pair_match` + SQL mirror, `near_name_spelling` merge options, `FinfluencerPairDismissal` migration 027, create-dialog warning, batched proof-segment `MERGE`); **Ads review** wrapped **2026-10-20** (shared Claude/Grok files; Grok 08:10 routine; ank applies approved ad changes); **Market Call** wrapped **2026-10-14** (14th `podcast_rss`; Pattern 8; directory Covered `market-call`); **Retiring LLM models** wrapped **2026-10-07** (closed Sonnet 4.5 / GPT-5.1 eval configs; refuse not remap); **Sector ETF registry** wrapped **2026-10-01** ([[concepts/sector-etf-registry]]); **Information Ratio** wrapped **2026-09-29** ([[concepts/information-ratio]]); **post-cutoff IPO resolution** wrapped **2026-09-28** ([[concepts/post-cutoff-ipo-resolution]]); **social share previews** wrapped **2026-09-28**; **finfluencer, show & ticker profile pages** wrapped **2026-09-28** (FR-1.11 top/bottom decile capture cancelled before build); **pipeline dashboard users screen** wrapped **2026-09-24** (test users in `internal_accounts` since 2026-09-25; onboarding columns); **Hedge Fund Tips with Tom Hayes** wrapped **2026-09-22** (359/359 downloaded; directory deferred); **Google Ads PMax creatives** wrapped **2026-09-04** (V1 + V4 live); **The Intrinsic Value Podcast** **2026-08-22**; **source quotes** + **LinkedIn outreach** **2026-08-11**; Gemini 3.5 Flash-Lite SI/FE **2026-07-27** |
| [[wiki/projects/gor-blog]] | finfluencer.trade | Active — **Factory `AGENTS.md` switch** **2026-11-12** (`exclude_docs: features/` verified; Market Hang & Schwab directory cards on `development`); F5Bot mailbox scoring live **2026-10-13** (Olli, `_internal/f5bot/`); **Cramer v1 closed** **2026-09-24**; platform-update post + Kit send **2026-08-14**; CTA pattern; apex `/api/subscribe` |
| [[wiki/projects/finfluencer-tracker]] | finfluencer.trade | Active — **Shows & finfluencer leaderboard horizon pick counts & floors** wrapped **2026-11-21** (`100+` shows via dependency-free `src/config/showLeaderboard.ts`, `20+` finfluencers on `/leaderboard` matching `IR_MIN_PICKS`; inline bootstrap URLs unchanged; live **2026-11-24** PR #148); **Search Console pull** plumbed **2026-11-20** (Monday 01:00 UTC workflow for Monday review; awaits `GSC_SERVICE_ACCOUNT_JSON`); **Backtest sign-up source** wrapped **2026-11-19** (`/backtest` explainer `signup_source` + `page_location` across magic link/OAuth; `BACKTEST_ROLLOUT` stays `'internal'`); **Under-the-radar pickers** wrapped **2026-11-19** (live since 2026-10-13; [[concepts/under-the-radar-pickers]]; guests see rank 1, signed-in accounts see the full list; 30-day row-open rate 4.0% vs 12.4% on `/leaderboard`); **Add to feed from leaderboard rows** wrapped **2026-11-19** (signed-in row button on four leaderboards, lazy layer, guests load nothing, `ListPlus`/`ListChecks` feed icons, `E9`–`E12`; live **2026-11-24** PR #148); **Feeds management** wrapped **2026-11-17** (Settings `+ New feed` & active-only `View in Picks`, inline browsable Who/What pickers via `feed_picker_counts`, 50-row scrollable `preview_feed` + star hint, `Create/Save and view picks` with GA4 `then`); **Feed email glimpse & self-contained `/api/feed-email`** wrapped **2026-11-17** ([[projects/finfluencer-tracker]]; self-contained CommonJS endpoint, deduplicated Top-5 stock cards, per-feed match stats, `/settings#feeds` footer); **Guest leaderboard table, mobile landing tour & magic-link resend** wrapped **2026-11-14**; **Factory `AGENTS.md` switch** **2026-11-12** (`docs/ops/{deployment-previews,performance-budget}.md`, `docs/design-system/agent-conventions.md`); **Share-class identity** wrapped **2026-11-10** ([[concepts/share-class-identity]]; seven companies, one ticker each; map in `gor_dagster`, empty table leaves RPCs unchanged); **Growth loop + marketing playbook** synced **2026-11-02** (`docs/ops/growth-loop/`); **Prediction Scores (`1★–5★`) & Option 2 3-tier gating** wrapped **2026-10-30** ([[concepts/prediction-scores]]; 10-session fresh window `get_fresh_scored_picks` on `/signals` & profile `<FreshScoredPicksSection />`; Guest sees 3 featured finfluencers on aggregate views with sort-order leak closed before `ORDER BY`/`ROW_NUMBER()` + public `/signals` with scores masked; Spectator unlocks 100% of aggregate track records across all 84 finfluencers & 14 shows + `1★–5★` scores for 3 featured finfluencers; Trader unlocks all fresh `1★–5★` scored picks across all 84 finfluencers); **landing tour** wrapped **2026-10-20** (50/50 on `/`; first read 6/103 vs 7/112; split stays; Thursday review); **RUM off GA4** **2026-10-20**; **mobile experience uplift** **2026-10-20**; **conversion measurement** wrapped **2026-10-20** (`sign_up` €20 is the only primary; other key events stay secondary); **signed-out QA findings and data integrity** wrapped **2026-10-19** (11-area sweep, 38 tasks: snapshot 31-day gate and Monday cron (2% history-drift / 20% period-move S&P guard 2026-11-23), SPY price repair, single-paid-panelist 80% masking, clean 404s, default `/compare` prefilled, Q4 pick count predicate with is_kept in sector averages, mobile signup uplift); **mobile sign-up drop** closed **2026-10-16** (Search mix, not the deploy; non-US cut and Search Partners; `paywall_view` / `paywall_cta_click` live); **Search QS landing page** wrapped **2026-10-14** (heading plus supporting line; lost IS rank 72.42% vs 86.82%; page copy stays); **ad landing time to content** wrapped **2026-10-09** (shell starts the guest request; server HTML stayed closed); **onboarding skip improvements** **2026-10-06** (completion 6/14; Skip stays; loading masks are not hit targets); **Supabase ledger baseline** **2026-09-30** (migrations replay from empty; histories repaired); **Information Ratio** on leaderboard and profiles **2026-09-29** ([[concepts/information-ratio]]); **profile script split** **2026-09-28** (show data-ready; stand-ins on `development`; body split not shipped); **onboarding questionnaire v2** **2026-09-28**; **feedback e2e on demand** **2026-09-28**; **investor positioning** **2026-09-27**; **Search RSA stills** **2026-09-27**; **performance palette** **2026-09-25**; **sector name filter at every width** **2026-09-24**; **user-default holding period** **2026-09-16**; **post-signup onboarding** **2026-09-09**; **field CWV + DATA_READY** **2026-09-08**; **finfluencer ticker lookup** **2026-09-05**; **Google Ads PMax creatives** **2026-09-04**; **GA4 instrumentation gap fixed + BigQuery export** **2026-09-02**; **mobile CWV** **2026-08-22**; **feedback Declined + admin note** **2026-08-22**; **Clarity session replay** **2026-08-18**; **Explore nav** **2026-08-17**; **Reddit Ads pixel** **2026-08-17**; **cumulative performance charts** **2026-08-04**; **subscription entitlement SSOT** **2026-07-27** |
| [[wiki/projects/rattaproff]] | rattaproff | Operational — **Slim Google Sheet migration** wrapped **2026-11-21** (65-col `RAW` sheet `1tWrtr3hSpwW-jgf-KBVm8oaWMF0ax9-FfC2uJrwXNHU` via `RATTAPROFF_GSHEET_ID`, `78 MiB` cap, permalinks in Cloud SQL, automated-translation purge); **Woo sync comparison and draft indexing** wrapped **2026-11-21** (`26,649 s` / 7.4h live run, `New: 2,596 / Updated: 5,022 / Deleted: 260`, 4 bounded Cyrillic slug retries `201`, `cykelpoint_dk` HTML `404` retry recovery, `7/7` post-run stores at `0/0/0`); category URL export **2026-08-14** |
| [[wiki/projects/ai-factory]] | (methodology) | Active — **Antigravity Gemini 4 & Orchestrator modes** wrapped **2026-11-17** (`gemini-4-hydrogen-medium` replacing `3.8-flash`, *Orchestrator (Carbon)* via `no_review`, effort-suffix lookup, [feature doc](file:///Users/andreskull/ai-factory/docs/architecture/features/antigravity-gemini4-models.md)); Self-switched **2026-11-13** (`REPO.md`, `CLAUDE.md` -> `@REPO.md`, `WIKI.md`); `<skill>:` composer prefix & Claude 2.1.323 / Fable 5.2 `author_only` **2026-11-13**; Phases 0–5 complete **2026-11-12** across all 7 switched repos |
| [[wiki/projects/spec-driven-ai-coding]] | (methodology) | Retired **2026-11-12** — superseded by [[projects/ai-factory]]; `~/.ai_global` removed & sync scripts guarded |
| [[wiki/projects/cramer-mad-money-research]] | finfluencer.trade | v1 frozen **2026-09-24** — public kit + SSRN 6643379; do not overwrite `data/` |
| [[wiki/projects/botastico-portal]] | botastico | Active — **`gpt-6-luna` temperature suppression & strict `chat_image_attachments_enabled === true` upload gate** **2026-11-19**; **Local widget Cypress harness, standalone `/chatapps` + `/iframe` isolation & `gpt-4o-mini` fallback** **2026-11-15** (`920477a`, `cab0e26` on `main`; 24/24 live Blurby + Opiq public URLs); **Factory `AGENTS.md` switch** **2026-11-12** (`development`, Turborepo Next.js `apps/client` `:3000` on Vercel `https://botasti.co`, `apps/admin`, `apps/playground`, `slack-events` Cloud Function) |
| [[wiki/projects/botastico-api]] | botastico | Active — **GPT-5.1 retirement and `gpt-6-luna` rollout** wrapped **2026-11-19** (`00089-tbp`, `20f09d1`; all 49 live production agents & 7 plans migrated to `gpt-6-luna` at 1 IA credit, 7 retired GPT-5/Claude 3 models removed from catalog, [feature doc](file:///Users/andreskull/botastico-api/docs/architecture/features/gpt-5-1-retirement.md)); **Deprecated OpenAI models removal** wrapped **2026-11-15** (`00087`; `o3-mini` -> `gpt-5.4-mini`, Claude 4.5 Haiku / Sonnet 4.6 added, live OpenAI deprecations gate, [feature doc](file:///Users/andreskull/botastico-api/docs/architecture/features/deprecated-openai-models-removal.md)); **Factory `AGENTS.md` switch + `gh-pages` docs leak closed** **2026-11-12** (`Agent branch: none` single-branch `main` unstaged-until-production); chat image attachments **2026-05-18** ([feature doc](file:///Users/andreskull/botastico-api/docs/architecture/features/botastico-chat-image-attachments.md)); Cloud Run Flask + `file_processor` |
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
| [[wiki/concepts/under-the-radar-pickers]] | finfluencer-tracker + gor_dagster: ranking by alpha on stocks outside the 100 most-mentioned at the time of the pick; 1M / 3M / 6M; guests see rank 1; signed-in accounts see the full list (2026-11-19) |
| [[wiki/concepts/prediction-scores]] | gor_dagster + tracker: ex-ante `1★–5★` 6M outperformance scorer (`blend_w50_v1`), per-cutoff `[T_k, T_{k+1})` model registry, monthly validation gate, automatic cutoff drift retraining (`>= 500` rows & `>= 5%`), `/scoring-models` dashboard, 10-session fresh window (`get_fresh_scored_picks`), and Option 2 3-tier gating (`Guest` → `Spectator` → `Trader`) (2026-10-30) |
| [[wiki/concepts/speaker-attribution]] | Two-stage LLM pipeline; production recursive SI (`si-haiku55-recursive` @450s, 2026-10-24); Stage 2 hydration O(U log W + W), FR-6 membership; ElevenLabs mono-speaker hardening |
| [[wiki/concepts/actionable-signal]] | Final output VIEW — speaker-gated; FE priority 1 = `fe-gem35fl-recursive` (2026-07-27; paired with Haiku 5.5 SI 2026-10-24) |
| [[wiki/concepts/signal-performance]] | Signal → horizons: truncation, implicit flip; SPY benchmark `BBG000BDTBL9`; as-of exit pricing (2026-06-30); uniform `next_market_open` across all 18 shows (2026-11-21) |
| [[wiki/concepts/llm-config-registry]] | Multi-model experimentation; Option 3 Hybrid Routing production `si-haiku55-recursive` / `fe-gem35fl-recursive` (2026-10-24) |
| [[wiki/concepts/spec-driven-development]] | Multi-model spec-driven development loop via [[projects/ai-factory]] across Claude Code, Cursor & Antigravity (Updated 2026-11-17) |
| [[wiki/concepts/huf-eur-pipeline-pricing]] | rattaproff: HUF sheet vs per-site EUR recompute for Woo diffs |
| [[wiki/concepts/gsheet-ground-truth-sync]] | rattaproff: 65-col `RAW` slim sheet (`1tWrtr3hSpwW-jgf-KBVm8oaWMF0ax9-FfC2uJrwXNHU`, `78 MiB` cap, 2026-11-21), grow-only chunked writes, `__sync_status` marker, GCS fallback |
| [[wiki/concepts/onboarding-new-podcast-source]] | gor_dagster: playbook for new RSS → ActionableSignal; directory PR required (Req 12); Pattern 8 Omny clip UUID + job-only `rel="next"` (2026-10-14); Pattern 7 PPLLC + slug freeze (2026-08-22); no idempotency requirement (retired 2026-08-19); iTunes feed discovery; SI auto-discovers podcast_rss |
| [[wiki/concepts/proof-segment-speaker-resolution]] | Proof-segment quote speakers → Finfluencer; ActionableSignal gate; display_name at sync (2026-07-01) |
| [[wiki/concepts/resolution-pipeline-efficiency]] | gor_dagster: JW matcher, re-attempt union, backlog sweeps, alias-on-resolve |
| [[wiki/concepts/curation-learning]] | gor_dagster: fund-noise similarity, unique-ticker bar 0.85, Stage 0.75 promotion (2026-07-07) |
| [[wiki/concepts/episode-title-speaker-hint]] | gor_dagster: authoritative episode-title guest spellings guide recursive SI bootstrap canonicalization, `decide_title_guard` auto-create guard, curation dashboard notes, 23 historical finfluencers corrected via reviewed manifest (2026-10-25) |
| [[wiki/concepts/foreign-listing-resolution]] | gor_dagster: native TSX (`.TO`) and TSXV (`.V`) matching, `choose_listing` policy, contained-name rule, 465 rows & 185 aliases repointed, 104 vacated FIGI-less OTC rows deleted (2026-10-25) |
| [[wiki/concepts/post-cutoff-ipo-resolution]] | gor_dagster: optional ticker, first EOD bar gate, `v_ticker_tenancy` for reused symbols (2026-09-28) |
| [[wiki/concepts/information-ratio]] | gor_dagster + tracker: kept-pick alpha / sample stddev, NULL below 20; replaced Sharpe (2026-09-29); `/leaderboard` filters `< 20` completed horizon picks (2026-11-21) |
| [[wiki/concepts/sector-etf-registry]] | gor_dagster: sector calls → concept ETF (GICS = benchmark ETFs); dismissed → registry → alias → human; Map to sector concept (2026-10-01) |
| [[wiki/concepts/linkedin-enrichment]] | gor_dagster + tracker: trust-tiered LinkedIn URLs; trusted-only Supabase sync; no third-party API (2026-07-23) |
| [[wiki/concepts/signal-source-quote]] | gor_dagster: `raw_source_quote` from proof_segments; blank beats approximate; backfill + FE forward fix (2026-08-11) |
| [[wiki/concepts/linkedin-outreach]] | gor_dagster + tracker: Notion Accepted drafts + optional chart MP4; human LinkedIn send only (2026-08-11) |
| [[wiki/concepts/google-ads-creative-assets]] | gor_dagster + tracker: monthly PMax images + video from the shipped chart encoder; ads subject period is 6 months (2026-09-27); Search RSA studio stills separate; coverage ranking; write-once GCS (2026-09-04) |
| [[wiki/concepts/share-class-identity]] | finfluencer-tracker + gor_dagster: seven companies are one ticker each; map in `utils/share_classes.py`, synced to `share_class_tickers`; a pick keeps its own ticker and return (2026-11-10) |
| [[wiki/concepts/finfluencer-ticker-pick-lookup]] | finfluencer-tracker: profile ticker lookup + ranked lists share `finfluencer_ticker_list`; typeahead is one option per canonical ticker (2026-11-10); PostgREST 1000-row cap; two box-plot charts retired (2026-09-05) |
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
| [[wiki/decisions/scribe-v3-default]] | gor_dagster: new transcriptions use ElevenLabs Scribe v3; v1 and v2 transcripts stay valid (2026-11-14) |
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
