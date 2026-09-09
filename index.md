---
type: index
title: "Wiki Index"
created: 2026-04-06
updated: 2026-09-09
---

# Wiki Index

Master catalog of all pages in this wiki. Update this file whenever a page is added, removed, or significantly renamed.

---

## Overview

- [[wiki/overview]] — Three products, one methodology, one wiki

---

## Products

| Page | Description |
|------|-------------|
| [[wiki/products/finfluencer-trade]] | Financial influencer accountability platform — planning hub (growth + MVP docs on product page) |
| [[wiki/products/rattaproff]] | WooCommerce multi-store automation — 20 storefronts; HUF→EUR pricing [[wiki/concepts/huf-eur-pipeline-pricing]]; gsheet sync safety [[wiki/concepts/gsheet-ground-truth-sync]]; category URL export (Woo slugs, 2026-08-14) |
| [[wiki/products/botastico]] | Partially indexed — [[projects/botastico]] SSL/monitoring (2026-07-15); [[projects/botastico-api]] (2026-05-18); other repos pending |

---

## Projects

| Page | Product | Status |
|------|---------|--------|
| [[wiki/projects/gor_dagster]] | finfluencer.trade | Active — **Google Ads PMax creatives** wrapped **2026-09-04** (V1 + V4 live); **The Intrinsic Value Podcast** **2026-08-22**; **source quotes** + **LinkedIn outreach** **2026-08-11**; Gemini 3.5 Flash-Lite SI/FE **2026-07-27** |
| [[wiki/projects/gor-blog]] | finfluencer.trade | Active — platform-update post + Kit send **2026-08-14**; directory Covered + Investing Unscripted **2026-07-27**; CTA pattern; apex `/api/subscribe`; `api/newsletter/` |
| [[wiki/projects/finfluencer-tracker]] | finfluencer.trade | Active — **post-signup onboarding** **2026-09-09**; **field CWV + DATA_READY** **2026-09-08**; **finfluencer ticker lookup** **2026-09-05**; **Google Ads PMax creatives** **2026-09-04**; **GA4 instrumentation gap fixed + BigQuery export** **2026-09-02**; **mobile CWV** **2026-08-22**; **feedback Declined + admin note** **2026-08-22**; **Clarity session replay** **2026-08-18**; **Explore nav** **2026-08-17**; **Reddit Ads pixel** **2026-08-17**; **cumulative performance charts** **2026-08-04**; **subscription entitlement SSOT** **2026-07-27** |
| [[wiki/projects/rattaproff]] | rattaproff | Operational — category URL export from Woo slugs **2026-08-14**; gsheet sync safety **2026-07-13**; permalink backfill + gsheet restore (Jun 2026); 20 storefronts |
| [[wiki/projects/spec-driven-ai-coding]] | (methodology) | Active |
| [[wiki/projects/cramer-mad-money-research]] | finfluencer.trade | Public kit + SSRN 6643379 — CSVs, scripts, paper |
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
| [[wiki/concepts/speaker-attribution]] | Two-stage LLM pipeline; production recursive SI (`si-gem35fl-recursive` @450s, 2026-07-27); Stage 2 hydration O(U log W + W), FR-6 membership; ElevenLabs mono-speaker hardening |
| [[wiki/concepts/actionable-signal]] | Final output VIEW — speaker-gated; FE priority 1 = `fe-gem35fl-recursive` (2026-07-27) |
| [[wiki/concepts/signal-performance]] | Signal → horizons: truncation, implicit flip; SPY benchmark `BBG000BDTBL9`; as-of exit pricing (2026-06-30) |
| [[wiki/concepts/llm-config-registry]] | Multi-model experimentation; production `si-gem35fl-recursive` / `fe-gem35fl-recursive` (2026-07-27) |
| [[wiki/concepts/spec-driven-development]] | The development methodology loop |
| [[wiki/concepts/huf-eur-pipeline-pricing]] | rattaproff: HUF sheet vs per-site EUR recompute for Woo diffs |
| [[wiki/concepts/gsheet-ground-truth-sync]] | rattaproff: grow-only sheet writes, `__sync_status` marker, GCS fallback (2026-07-13) |
| [[wiki/concepts/onboarding-new-podcast-source]] | gor_dagster: playbook for new RSS → ActionableSignal; directory PR required (Req 12); Pattern 7 PPLLC + slug freeze (2026-08-22); no idempotency requirement (retired 2026-08-19); iTunes feed discovery; SI auto-discovers podcast_rss |
| [[wiki/concepts/proof-segment-speaker-resolution]] | Proof-segment quote speakers → Finfluencer; ActionableSignal gate; display_name at sync (2026-07-01) |
| [[wiki/concepts/resolution-pipeline-efficiency]] | gor_dagster: JW matcher, re-attempt union, backlog sweeps, alias-on-resolve |
| [[wiki/concepts/curation-learning]] | gor_dagster: fund-noise similarity, unique-ticker bar 0.85, Stage 0.75 promotion (2026-07-07) |
| [[wiki/concepts/linkedin-enrichment]] | gor_dagster + tracker: trust-tiered LinkedIn URLs; trusted-only Supabase sync; no third-party API (2026-07-23) |
| [[wiki/concepts/signal-source-quote]] | gor_dagster: `raw_source_quote` from proof_segments; blank beats approximate; backfill + FE forward fix (2026-08-11) |
| [[wiki/concepts/linkedin-outreach]] | gor_dagster + tracker: Notion Accepted drafts + optional chart MP4; human LinkedIn send only (2026-08-11) |
| [[wiki/concepts/google-ads-creative-assets]] | gor_dagster + tracker: monthly PMax images + video from the shipped chart encoder; coverage ranking; write-once GCS (2026-09-04) |
| [[wiki/concepts/finfluencer-ticker-pick-lookup]] | finfluencer-tracker: profile ticker lookup + ranked lists share `finfluencer_ticker_list`; PostgREST 1000-row cap; two box-plot charts retired (2026-09-05) |
| [[wiki/concepts/subscription-entitlement-ssot]] | finfluencer-tracker: profile tier = app entitlement; Stripe billing-only; reconcile + RLS close (2026-07-27) |
| [[wiki/concepts/cumulative-performance-charts]] | finfluencer-tracker: cumulative % profile + `/compare` + watermarked export; SPY sync; waypoint-shaped path (2026-08-04) |
| [[wiki/concepts/botastico-ssl-certificates]] | botastico: GCP LB managed certs, auto-renew, renewal-failure monitoring (2026-07-15) |
| [[wiki/concepts/google-ads-conversion-tracking]] | finfluencers.trade paid measurement — ONE Google tag `G-BLE3H05Q5T` → GA4 `485294334` + `AW-18322362149`; legacy "Finfluencers.Bet" name renamed 2026-08-04; trust IDs not names |
| [[wiki/concepts/reddit-ads-conversion-tracking]] | finfluencers.trade Reddit pixel — `SignUp` + `PageVisit`, consent gate before `pixel.js`; Conversions campaign live, Traffic Max paused (2026-08-17) |
| [[wiki/concepts/session-replay-analytics]] | finfluencers.trade Microsoft Clarity — Production-only replay; analytics consent; Balanced + Settings mask; funnel events never Ads conversions (2026-08-18) |
| [[wiki/concepts/post-signup-onboarding]] | finfluencer-tracker: auth-callback onboarding detour; skippable wizard; funnel counts in GA4 not Clarity (2026-09-09) |
| [[wiki/concepts/feedback-roadmap]] | finfluencer-tracker native `/feedback` board — Declined + public admin note; `notify_requested_at` intent marker; vote allowlist; never rewrite `handle_feedback_notification()` (2026-08-22) |
| [[wiki/concepts/core-web-vitals-field]] | finfluencer-tracker first-party field RUM + DATA_READY; field decides done; `/` teaser still ~4 s (2026-09-08) |
| [[wiki/concepts/core-web-vitals-mobile]] | finfluencer-tracker SPA delivery — lab LCP under 2.5 s on `/`; Search Console group half MkDocs (2026-08-22; lab-as-gate amended 2026-09-08) |

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
| [[wiki/decisions/botastico-gcp-managed-ssl-2026-07]] | botastico: stay on GCP managed LB certs; alert on renewal failure only (2026-07-15) |
| [[wiki/decisions/exclude-regulated-finance-employees-from-outreach-2026-08]] | finfluencer.trade: qualify outreach by employer; hold contacts at registered entities from automated performance messaging (2026-08-31) |
| [[wiki/decisions/ga4-instrumentation-registration-2026-09]] | finfluencer-tracker: register 10 GA4 custom dimensions + `metric_value` metric, link BigQuery export to `gurus-on-record` (US multi-region, kept) after finding registration was never done (2026-09-02) |
| [[wiki/decisions/field-authoritative-cwv-2026-09]] | finfluencer-tracker: field first-party p75 decides CWV done; lab decides whether a change helped (2026-09-08) |

---

## Synthesis

| Page | Description |
|------|-------------|
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
