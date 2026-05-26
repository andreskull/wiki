---
type: index
title: "Wiki Index"
created: 2026-04-06
updated: 2026-05-18
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
| [[wiki/projects/gor_dagster]] | finfluencer.trade | Active — **BigQuery cost program** wrapped **2026-05-17** ([bigquery-cost-optimization.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/bigquery-cost-optimization.md)); six SI-monitored podcasts; ContentItem dedupe; DeepSeek SI/FE; pytest not-expensive green |
| [[wiki/projects/gor-blog]] | finfluencer.trade | Active — CTA pattern shipped (Cramer post); apex `/api/subscribe` via landing rewrite; `api/newsletter/`; `research/cramer/` |
| [[wiki/projects/finfluencer-tracker]] | finfluencer.trade | Active — `docs/architecture/` in repo |
| [[wiki/projects/rattaproff]] | rattaproff | Operational — HUF/EUR pricing doc + `_build_supplier_huf_base_df` fix (May 2026) |
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
| [[wiki/concepts/speaker-attribution]] | Two-stage LLM pipeline for naming podcast speakers |
| [[wiki/concepts/actionable-signal]] | Final output VIEW — speaker-gated, not a table |
| [[wiki/concepts/signal-performance]] | Signal → horizons: truncation, implicit opposite-direction close (same instrument) |
| [[wiki/concepts/llm-config-registry]] | How multi-model experimentation is managed |
| [[wiki/concepts/spec-driven-development]] | The development methodology loop |
| [[wiki/concepts/huf-eur-pipeline-pricing]] | rattaproff: HUF sheet vs per-site EUR recompute for Woo diffs |
| [[wiki/concepts/onboarding-new-podcast-source]] | gor_dagster: playbook for new RSS → ActionableSignal (gates, PRs); links to repo operations doc |

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
