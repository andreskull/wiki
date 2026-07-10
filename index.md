---
type: index
title: "Wiki Index"
created: 2026-04-06
updated: 2026-07-10
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
| [[wiki/products/rattaproff]] | WooCommerce multi-store automation — 7 storefronts; HUF→EUR pricing concept [[wiki/concepts/huf-eur-pipeline-pricing]] |
| [[wiki/products/botastico]] | Partially indexed — [[projects/botastico-api]] (2026-05-18); other repos pending |

---

## Projects

| Page | Product | Status |
|------|---------|--------|
| [[wiki/projects/gor_dagster]] | finfluencer.trade | Active — **Recursive LLM extraction** wrapped **2026-07-01** (`si-gem31fl-recursive` / `fe-gem31fl-recursive`; batch Phase 5 → `batch-integration`); proof-segment gate **2026-07-01**; Hidden Gems **2026-07-01**; post-cutoff IPO backfill gate **2026-06-28** |
| [[wiki/projects/gor-blog]] | finfluencer.trade | Active — CTA pattern shipped (Cramer post); apex `/api/subscribe` via landing rewrite; `api/newsletter/`; `research/cramer/` |
| [[wiki/projects/finfluencer-tracker]] | finfluencer.trade | Active — **landing conversion funnel** wrapped **2026-07-10** (`docs/architecture/features/landing-conversion-improvements.md`; public leaderboards + profile gates) |
| [[wiki/projects/rattaproff]] | rattaproff | Operational — permalink backfill + gsheet restore complete (Jun 2026); 20 storefronts; `docs/architecture/features/permalink-redirect-resolution.md` |
| [[wiki/projects/spec-driven-ai-coding]] | (methodology) | Active |
| [[wiki/projects/cramer-mad-money-research]] | finfluencer.trade | Public kit + SSRN 6643379 — CSVs, scripts, paper |
| [[wiki/projects/botastico-api]] | botastico | Active — chat image attachments **2026-05-18** ([feature doc](file:///Users/andreskull/botastico-api/docs/architecture/features/botastico-chat-image-attachments.md)); Cloud Run Flask; `slack_chat_logs` Pub/Sub consumer |

---

## Sources

| Page | Description |
|------|-------------|
| [[wiki/sources/2026-04-07-cramer-mad-money-performance-methodology]] | Cramer / Mad Money performance & methodology ([[projects/cramer-mad-money-research]]) |

---

## Concepts

| Page | Description |
|------|-------------|
| [[wiki/concepts/speaker-attribution]] | Two-stage LLM pipeline; production recursive SI (`si-gem31fl-recursive`); Stage 2 hydration O(U log W + W), FR-6 membership; ElevenLabs mono-speaker hardening (2026-07-01) |
| [[wiki/concepts/actionable-signal]] | Final output VIEW — speaker-gated, not a table |
| [[wiki/concepts/signal-performance]] | Signal → horizons: truncation, implicit flip; SPY benchmark `BBG000BDTBL9`; as-of exit pricing (2026-06-30) |
| [[wiki/concepts/llm-config-registry]] | Multi-model experimentation; production `si-gem31fl-recursive` / `fe-gem31fl-recursive` (2026-07-01) |
| [[wiki/concepts/spec-driven-development]] | The development methodology loop |
| [[wiki/concepts/huf-eur-pipeline-pricing]] | rattaproff: HUF sheet vs per-site EUR recompute for Woo diffs |
| [[wiki/concepts/onboarding-new-podcast-source]] | gor_dagster: playbook for new RSS → ActionableSignal (gates, PRs); links to repo operations doc |
| [[wiki/concepts/proof-segment-speaker-resolution]] | Proof-segment quote speakers → Finfluencer; ActionableSignal gate; display_name at sync (2026-07-01) |
| [[wiki/concepts/resolution-pipeline-efficiency]] | gor_dagster: JW matcher, re-attempt union, backlog sweeps, alias-on-resolve |
| [[wiki/concepts/curation-learning]] | gor_dagster: fund-noise similarity, unique-ticker bar 0.85, Stage 0.75 promotion (2026-07-07) |

---

## Entities

| Page | Description |
|------|-------------|
| [[wiki/entities/bigquery]] | Google Cloud data warehouse — used across products |
| [[wiki/entities/dagster]] | Data orchestration — backbone of gor_dagster |
| [[wiki/entities/gcs]] | Object storage — media and STT transcripts (gor_dagster) |

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
