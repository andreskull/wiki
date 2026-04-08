---
type: overview
title: "Wiki Overview"
created: 2026-04-06
updated: 2026-04-08
---

# Wiki Overview

Three products, one methodology, one wiki. Updated: 2026-04-08.

## Products

**[[products/finfluencer-trade]]** — Financial influencer accountability platform. Ingests podcast content, transcribes it, extracts stock picks, tracks prediction performance. Three repos: `gor_dagster` (data pipeline — the most complex system in the portfolio), `gor-blog` (public site and published research), `finfluencer-tracker` (auth/landing layer). Pipeline is production-ready; active development ongoing across 7 features in `gor_dagster`.

**[[products/rattaproff]]** — WooCommerce multi-store automation for a network of 7 e-commerce storefronts. Single repo. Handles supplier ingestion, pricing, publishing, and Google Indexing API quota management. Operational but docs sparse.

**[[products/botastico]]** — Not yet indexed (repos not accessible during bootstrap). 7 repos. To be added in a future pass.

## Methodology

**[[projects/spec-driven-ai-coding]]** — The development process used across all projects. Spec → Plan → Execute → Wrapup. Provides global `/` commands for Cursor and Antigravity. The `/wrapup` command is the bridge between completed features and the wiki.

## Active development (as of bootstrap)

`gor_dagster` has 7 features in progress: `batch-integration`, `finfluencer-affiliations-human-curation`, `instrument-resolution-bulk-manual-curation`, `pipeline-model-priority-retries`, `post-mvp-loops-migration`, `proof-segment-speaker-resolution`, `social-share-previews`. None yet wrapped up — their docs are temporary and not indexed here.

## Key concepts to know

- [[concepts/speaker-attribution]] — two-stage LLM pipeline for naming podcast speakers
- [[concepts/actionable-signal]] — the final output; a VIEW not a table; speaker-gated
- [[concepts/signal-performance]] — how picks become measured horizons; truncation + implicit flip on same instrument
- [[concepts/llm-config-registry]] — how multi-model experimentation is managed
- [[concepts/spec-driven-development]] — the development methodology

## Open threads

- Botastico repos not yet indexed
- `rattaproff` docs sparse — sync when more content exists; **`finfluencer-tracker`** — run **`wiki sync finfluencer-tracker`** after changes to **`WIKI.md`** or **`docs/`** (excluding `docs/features/`)
- 7 gor_dagster features in progress — wiki will grow as each wraps up
