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

## [2026-04-27] sync | finfluencer-trade — Documented durable planning locations on [[wiki/products/finfluencer-trade]] (`growth_plan.md`, `MVP_MASTER_PLAN.md`, `INCR_01_MASTER_PLAN.md`, app runbook); explicit note to **not** use `gor_dagster/docs/features/<feature>/` for long-term planning. Cross-links on [[wiki/projects/gor-blog]], [[wiki/projects/gor_dagster]], [[wiki/projects/finfluencer-tracker]]; [[wiki/overview]], [[index.md]].

## [2026-04-29] sync | gor_dagster — Corrected BigQuery dataset ownership: `SignalPerformance`, `PriceHistory`, `TradingCalendar`, and performance aggregate views live in `dagster_prices`, not `dagster_shared`. Updated [[wiki/projects/gor_dagster]], [[wiki/entities/bigquery]], repo `WIKI.md`, `docs/architecture/core-data-model.md`, [[index.md]].

## [2026-04-29] sync | gor-blog — Newsletter: registered users → Kit import wrapped (`import_registered_users.py`, `registered-users-kit-import.md`); `docs/features/users-to-newsletter-subscribers/` removed; `convertkit_template_final.html` + logo/Gmail dark-mode work; `newsletter_preview.html` gitignored. Updated repo `WIKI.md`, `api/newsletter/README.md`, [[wiki/projects/gor-blog]], [[wiki/overview]] (date), [[index.md]].

## [2026-05-05] sync | gor-blog — Established blog post CTA pattern after Cramer launch funnel analysis (92% article-to-product abandonment, uniform across acquisition channels — Reddit 0/13, LinkedIn 0/2, Facebook 0/1; verified via GA4 Funnel exploration with Page referrer breakdown, May 4 2026). New concept page [[wiki/concepts/blog-post-cta-pattern]] codifies four required CTAs on every post (above-fold module, mid-article module, expanded end-of-post block, internal product links) plus two template-level optional elements. Updated [[wiki/projects/gor-blog]] (new "Required elements on every blog post" section), [[index.md]]. Source data in `gor-blog/research/cramer/promotion/linkedin_promotion_plan.md` § *Day 1 learnings + plan revision*.

## [2026-05-05] sync | gor-blog — Established gor-blog-specific feature folder convention: feature specs (requirements / design / tasks) live at `gor-blog/features/<feature-name>/`, NOT `gor-blog/docs/features/`. Reason: `gor-blog/docs/` is the published MkDocs site root, so anything under it is exposed on `finfluencers.trade`. This is a deviation from the `docs/features/` convention used in `gor_dagster`, `finfluencer-tracker`, and `rattaproff`. Updated [[wiki/projects/gor-blog]] § "Important structural note" with the new tree diagram and rationale. First feature using the new path: `gor-blog/features/cramer-post-cta-pattern/` (implements [[wiki/concepts/blog-post-cta-pattern]] on the existing Cramer post).

## [2026-05-05] sync | gor_dagster — `signal-pruning-performance` wrapup: removed `docs/features/signal-pruning-performance/` and temp parity scripts; `SignalPerformance_baseline.sql` retired from repo. Updated repo `WIKI.md`, `docs/schemas/README.md` (`dagster_prices` inventory). Wiki: [[wiki/projects/gor_dagster]] (Stage 7 pruning + hybrid serving, prices table incl. `_pruned` / baseline retired, completed-features row, active-features list aligned to repo `WIKI.md`), [[index.md]].

## [2026-05-05] sync | gor_dagster — housekeeping: removed `configs/prod_supabase_pruning_cutover_full_refresh.yaml` (cutover done), `scripts/drop_signal_performance_baseline.sql`, and ad-hoc pricing/Cramer diagnostic scripts from repo tree; docs describe prod full-refresh as ad hoc Launchpad config mirroring dev YAML. Wiki aligned (baseline row + Stage 7).

## [2026-05-06] sync | gor-blog — Renamed `research/cramer/promotion/linkedin_promotion_plan.md` → `cramer-study-launch-campaign.md` (cross-channel campaign log; old name implied LinkedIn-only). `growth_plan.md` § *Cramer Paper Promotion* links to the campaign file; `research/cramer/promotion/README.md` table updated. Updated [[wiki/projects/gor-blog]], [[wiki/concepts/blog-post-cta-pattern]], feature refs under `features/cramer-linkedin-promotion/`.

## [2026-05-12] sync | gor_dagster — Morning Filter wrapup: `docs/architecture/features/morning-filter-ingestion.md`, repo `WIKI.md` + README third source; `docs/features/morning-filter-ingestion/` removed. Wiki: [[projects/gor_dagster]], [[index.md]].

## [2026-05-06] sync | gor-blog — Removed redirect stub `linkedin_promotion_plan.md` (canonical campaign path: `cramer-study-launch-campaign.md`). Updated repo `WIKI.md` and [[wiki/projects/gor-blog]].

## [2026-05-13] sync | gor_dagster — Indexed [onboarding-new-podcast-source.md](file:///Users/andreskull/gor_dagster/docs/operations/onboarding-new-podcast-source.md): new wiki concept [[wiki/concepts/onboarding-new-podcast-source]], [[wiki/projects/gor_dagster]] (Stage 1 docs line, operations table, related pages), [[wiki/overview]], [[index.md]].

## [2026-05-14] sync | gor_dagster — Compound and Friends: `si_sensor` union now six shows (`MORNING_FILTER` + `COMPOUND_AND_FRIENDS` constants); README + repo `WIKI.md`; [[wiki/projects/gor_dagster]] Stage 1 catalogue + `PIPELINE_SI_MONITORED` link; feature `tasks.md` PR 5 monitoring removed per operator preference. Updated [[index.md]].

## [2026-05-14] sync | gor_dagster — DeepSeek-V4-Flash migration Task 15 closure: deleted `docs/features/deepseek-v4-flash-migration/`; removed temp scripts `diag_episode_hydration_si.py`, `find_low_coverage_episodes.py`, `find_task22_6_eligible_episodes.py`, Task 22 ID list files under `scripts/data/`; added `scripts/verify_dsv4fr_pp_actionable_signal.py`; **`docs/architecture/features/deepseek-v4-flash-migration.md`** now holds BQ counts + grok string-sweep acceptance; repo **`WIKI.md`**. Wiki: **`[[projects/gor_dagster]]`**, **`[[concepts/actionable-signal]]`**, **`[[index.md]]`**.

## [2026-05-14] wrapup | gor_dagster | Compound and Friends ingestion — added **`docs/architecture/features/compound-and-friends-ingestion.md`**, `docs/architecture/README.md` completed-feature link; updated **`onboarding-new-podcast-source.md`** + pytest green-track cross-link; **`docs/features/compound-and-friends-ingestion/`** removed. Repo **`WIKI.md`** (completed table + current status). Wiki: **`[[projects/gor_dagster]]`** (Stage 1 doc link, completed + active tables), **`[[index.md]]`**.

## [2026-05-15] wrapup | gor_dagster | compound-and-friends-ingestion — deleted residual **`docs/features/compound-and-friends-ingestion/`** (`requirements.md`, `design.md`, `tasks.md`); permanent record unchanged at **`docs/architecture/features/compound-and-friends-ingestion.md`**. Repo **`WIKI.md`** `Last updated`. Wiki **`[[projects/gor_dagster]]`** frontmatter `updated`.

## [2026-05-15] wrapup | gor_dagster | pytest-not-expensive-green — **`docs/architecture/features/pytest-not-expensive-green.md`** (DoD, env, landed fixes, **`pytest.ini`** ignore debt); **`docs/features/pytest-not-expensive-green/`** removed. Repo **`WIKI.md`** + **`docs/architecture/README.md`**. Wiki **`[[projects/gor_dagster]]`**, **`[[index.md]]`**.

## [2026-05-17] wrapup | gor_dagster | BigQuery cost optimization — **`docs/architecture/features/bigquery-cost-optimization.md`**; **`docs/operations/cost-snapshots/`** + **`docs/operations/bigquery-cost-findings/`** + **`docs/operations/PotentialPrediction-call-site-inventory.md`** relocated from removed **`docs/features/bigquery-cost-optimization/`**; **`scripts/bq_cost_report.py`** default out dir updated. Repo **`WIKI.md`**. Wiki **`[[projects/gor_dagster]]`**, **`[[index.md]]`**.

## [2026-05-18] sync | botastico-api — Repo **`WIKI.md`** + **`docs/architecture/features/botastico-chat-image-attachments.md`** (wrapped feature). New **`[[projects/botastico-api]]`**; **`[[products/botastico]]`** no longer pure stub (API indexed; `slack_chat_logs` path). **`[[wiki/entities/gcs]]`** + **`[[wiki/overview]]`**, **`[[index.md]]`**.

## [2026-05-31] wrapup | gor_dagster | 7investing Ingestion — added **`docs/architecture/features/7investing-ingestion.md`** and updated **`WIKI.md`**; registered UUID `c2658090-942b-4cbd-9552-f04995220873` in **`si_sensor.py`**; updated config validator to allow single-job execution (`total_jobs=1`); verified E2E and idempotency on production; deleted **`docs/features/7investing-ingestion/`** specs. Wiki project page `[[projects/gor_dagster]]` updated.

