# Wiki Log

Append-only. Grep recent entries: `grep "^## \[" log.md | tail -10`

---

## [2026-04-06] bootstrap | Initial wiki created — foundation files, product/project pages for accessible repos (gor_dagster, gor-blog, finfluencer-tracker, rattaproff, spec-driven-ai-coding). Botastico repos pending.

## [2026-04-06] sync | gor_dagster — Full rewrite of project wiki page. Now covers: all 7 pipeline stages in detail, full data model (15 BigQuery entities + 6 operational tables), GCS storage structure, LLM config registry, dynamic token allocation, pipeline model priority/retries, golden reference system, Finfluencer pre-seeding, Supabase sync layer (planned), monitoring dashboard, full deployment/infrastructure setup, complete code structure, all architecture decisions table, operations reference table with 15 guides.

## [2026-04-06] lint | finfluencer-trade — Corrected [[entities/bigquery]] dataset name (`dagster_shared`). Added [[entities/gcs]]. Synthesis: [[wiki/synthesis/lint-finfluencer-trade-2026-04-06]].

## [2026-04-06] sync | finfluencer-trade — BigQuery datasets corrected against live `bq ls` on `gurus-on-record`: `dagster_prod` = ContentSource, ContentItem, ContentSourceTimingConfig only; `dagster_shared` = pipeline tables/views. Updated [[entities/bigquery]], [[projects/gor_dagster]], [[products/finfluencer-trade]].

## [2026-04-06] sync | finfluencer-trade — Clarified: all backend (BigQuery/GCS) uses production assets; [[projects/finfluencer-tracker]] uses two Supabase instances (prod + dev). Updated [[products/finfluencer-trade]], [[projects/finfluencer-tracker]], [[wiki/synthesis/lint-finfluencer-trade-2026-04-06]].

## [2026-04-06] sync | finfluencer-tracker — [[wiki/synthesis/lint-finfluencer-trade-2026-04-06]]: resolved WIKI.md entry point; remaining action = stub wiki page until repo `docs/architecture/`. Updated [[projects/finfluencer-tracker]], [[wiki/overview]], repo `WIKI.md`.

## [2026-04-06] sync | gor-blog — Hosting documented as **Vercel** on [[projects/gor-blog]], repo `WIKI.md`, and [[wiki/synthesis/lint-finfluencer-trade-2026-04-06]] (removed incorrect “inferred / Cloudflare / GitHub Pages” follow-up).

## [2026-04-06] sync | finfluencer-tracker — Added `docs/architecture/` (README, system-overview, data-layer); updated `WIKI.md`, [[projects/finfluencer-tracker]], [[wiki/synthesis/lint-finfluencer-trade-2026-04-06]].

## [2026-04-06] lint | gor-blog — Verified 14 posts, empty `docs/features/`, Vercel stack. Fixed `gor-blog/WIKI.md` (vault indexes `docs/blog/posts/` as content outputs). Updated [[index.md]], [[wiki/synthesis/lint-gor-blog-2026-04-06]].

## [2026-04-06] lint | finfluencer-tracker — Aligned repo `WIKI.md` + [[wiki/projects/finfluencer-tracker]] with vault index rule (`docs/` except `docs/features/`). [[wiki/synthesis/lint-finfluencer-tracker-2026-04-06]], [[wiki/overview]].

## [2026-04-07] ingest | gor-blog `_internal/papers` — Archived Cramer methodology to `wiki/raw/papers/`, source [[wiki/sources/2026-04-07-cramer-mad-money-performance-methodology]]. Updated [[projects/gor-blog]], `gor-blog/WIKI.md`, [[index.md]], [[wiki/synthesis/lint-gor-blog-internal-papers-2026-04-07]].

## [2026-04-07] sync | gor-blog — Cramer paper moved to `gor-blog/research/cramer/`. Refreshed `wiki/raw/papers/` copy; updated [[wiki/sources/2026-04-07-cramer-mad-money-performance-methodology]], [[projects/gor-blog]], `gor-blog/WIKI.md`, [[wiki/synthesis/lint-gor-blog-internal-papers-2026-04-07]], [[index.md]].

## [2026-04-08] sync | gor_dagster — Signal→performance: documented implicit position-flip truncation in `docs/architecture/performance-methodology.md` + `docs/architecture/README.md`; repo `WIKI.md` bullet. New wiki concept [[wiki/concepts/signal-performance]]; updated [[wiki/concepts/actionable-signal]], [[wiki/projects/gor_dagster]] (Stage 7, decisions, Supabase sync factual), [[wiki/overview]], [[index.md]].

## [2026-04-16] sync | cramer-mad-money-research — New wiki project [[wiki/projects/cramer-mad-money-research]]; updated [[wiki/products/finfluencer-trade]], [[wiki/projects/gor-blog]], [[wiki/sources/2026-04-07-cramer-mad-money-performance-methodology]], [[wiki/overview]], [[index.md]]. Public repo ignores `WIKI.md`, `wiki/`, `.obsidian/` for vault safety.

## [2026-04-17] sync | gor-blog + cramer research layout — Clarified split: public [[wiki/projects/cramer-mad-money-research]] (CSVs + analysis-only scripts) vs private `gor-blog/research/cramer/` (BigQuery export scripts, `research_plan.md`, `SPEC_sequence_model.md`). Updated [[wiki/projects/gor-blog]], [[wiki/projects/cramer-mad-money-research]], [[wiki/sources/2026-04-07-cramer-mad-money-performance-methodology]], [[wiki/products/finfluencer-trade]], [[wiki/overview]], [[index.md]], [[wiki/synthesis/lint-gor-blog-internal-papers-2026-04-07]]; `gor-blog/WIKI.md` research section.

## [2026-04-25] sync | gor_dagster — Removed `docs/features/dagster-cloud-credit-optimization/` after wrapup; repo links now target `docs/architecture/features/dagster-cloud-credit-optimization.md` (incl. bigquery-cost-optimization cross-refs). Updated [[wiki/projects/gor_dagster]] (hydration sensor reality, completed-features table, deployment bullets, ops links), [[wiki/overview]], [[index.md]].

## [2026-04-25] sync | cramer-mad-money-research + gor-blog/research/cramer — Working paper on **SSRN** [6643379](https://ssrn.com/abstract=6643379); public GitHub kit aligned. Updated [[wiki/projects/cramer-mad-money-research]] (title, SSRN, status, tech, split with private folder), [[wiki/projects/gor-blog]] (`research/cramer/` layout: scripts, specs, `SSRN_submission.md`), [[wiki/sources/2026-04-07-cramer-mad-money-performance-methodology]], [[wiki/products/finfluencer-trade]], [[wiki/overview]], [[index.md]].

## [2026-04-27] sync | gor-blog `research/` — Removed `cramer/extras/` (QQQ-era scripts; in git history). Added `research/README.md`. Git index aligned. Updated [[wiki/projects/gor-blog]], [[wiki/sources/2026-04-07-cramer-mad-money-performance-methodology]], [[wiki/projects/cramer-mad-money-research]] (table row), [[log.md]].
