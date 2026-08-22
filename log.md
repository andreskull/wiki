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

## [2026-06-02] wrapup | finfluencer-tracker | Custom Feedback & Roadmap — Created in-repo architectural spec docs/architecture/feedback-system.md; configured database triggers for atomic upvotes and creator vote auto-seeding; automated Resend email moderation alerts and creator receipts via notify-moderators Deno Edge Function. Integrated URL tab deep-linking, Radix UI responsive widgets, and refined mobile views. Seeded 7investing and pick-ranking/top-ranked alert models directly into the Production database. Removed temporary feature development folder. Wiki project page [[projects/finfluencer-tracker]] updated.

## [2026-06-10] wrapup | gor_dagster | Resolution pipeline efficiency — permanent doc `docs/architecture/features/resolution-pipeline-efficiency.md`; JW matcher, re-attempt union, backlog sweeps, alias-on-resolve; 237 instrument + 18 speaker hashes unlocked; deleted `docs/features/resolution-pipeline-efficiency/` and gate/diagnostic scripts.

## [2026-06-10] sync | gor_dagster — Updated [[projects/gor_dagster]] Stage 6 resolution section, completed-features table, architecture decisions. New concept [[concepts/resolution-pipeline-efficiency]]. Linked from [[concepts/actionable-signal]]. Updated [[overview]] and [[index]].

## [2026-06-15] wrapup | gor_dagster | Hydration performance optimization — permanent doc `docs/architecture/features/hydration-performance-optimization.md`; canonical utils in `transcript_hydration_utils.py`; O(U·W)→O(U log W + W); FR-6 membership `MEMBERSHIP_TOL=0.01`; deleted `docs/features/hydration-performance-optimization/`.

## [2026-06-15] sync | gor_dagster — Updated [[projects/gor_dagster]] Stage 4b hydration section, completed-features table, architecture decisions. Expanded [[concepts/speaker-attribution]] Stage 2. Updated [[overview]] and [[index]].

## [2026-06-15] wrapup | gor_dagster | The Acquirers Podcast ingestion — permanent doc `docs/architecture/features/acquirers-podcast-ingestion.md`; 436/436 downloaded; PR #134 SI sensor; Patterns 4+5; deleted `docs/features/acquirers-podcast-ingestion/`.

## [2026-06-15] sync | gor_dagster — Updated [[projects/gor_dagster]] RSS catalogue (eight SI sources), completed-features table. Updated [[index]].

## [2026-06-28] wrapup | rattaproff | Permalink redirect resolution + gsheet disaster recovery — updated `docs/architecture/features/permalink-redirect-resolution.md`; gsheet restored to 42,221 rows; hardened `storage.df_to_storage` CSV quoting; removed investigation scripts/artifacts.

## [2026-06-28] sync | rattaproff — Updated [[projects/rattaproff]] (20 sites, permalink module, recovery ops). Updated [[index]].

## [2026-06-30] wrapup | gor_dagster | SPY canonical FIGI consolidation — permanent doc `docs/architecture/features/spy-canonical-figi-consolidation.md`; ops runbook `docs/operations/spy-figi-consolidation-runbook.md`; prod flip to `BBG000BDTBL9`; migration scripts removed; `spy_single_identity` asset check + verify script retained; deleted `docs/features/spy-canonical-figi-consolidation/`. Repo `WIKI.md` updated.

## [2026-06-30] sync | gor_dagster — Updated [[projects/gor_dagster]] (Stage 6 dashboard/SPY identity, Stage 7 as-of exit + benchmark, completed-features table, architecture decisions, active features, ops reference). Updated [[concepts/signal-performance]] (as-of exit, SPY FIGI). Updated [[overview]], [[index]].

## [2026-07-01] wrapup | gor_dagster | Motley Fool Hidden Gems Investing — permanent doc `docs/architecture/features/hidden-gems-ingestion.md`; Pattern 6 ARML; Megaphone slug validator; RSS dedupe + load-job idempotency; source-agnostic `si_sensor`; 2247/2247 downloaded; deleted `docs/features/motley-fool-hidden-gems-ingestion/`. Repo `WIKI.md` updated.

## [2026-07-01] sync | gor_dagster — Updated [[projects/gor_dagster]] (nine-source RSS catalogue, Pattern 6, source-agnostic SI, completed-features table, architecture decisions, wrapped feature note). Updated [[concepts/onboarding-new-podcast-source]] (Gate 2 resolved for podcast_rss). Updated [[overview]], [[index]].

## [2026-07-01] sync | gor_dagster — ElevenLabs mono-speaker SI hardening: resplit-at-unify, auto-heal at SI load (`elevenlabs_unified_heal.py`), healable stuck exclusion, merged-segment coverage fix. Updated repo `WIKI.md`, `docs/architecture/features/hidden-gems-ingestion.md`, `docs/architecture/core-data-model.md`. Wiki: [[projects/gor_dagster]] (Stage 3–4, architecture decisions), [[concepts/speaker-attribution]], [[overview]], [[index]].

## [2026-07-01] wrapup | gor_dagster | Proof-segment speaker resolution — permanent doc `docs/architecture/features/proof-segment-speaker-resolution.md` updated (2026-07-01 operational verification); deleted `docs/features/proof-segment-speaker-resolution/`; ops scripts retained. Repo `WIKI.md` updated.

## [2026-07-01] sync | gor_dagster — Proof-segment speaker resolution wrapup: new [[concepts/proof-segment-speaker-resolution]]; updated [[projects/gor_dagster]] (Stage 6, ActionableSignal gate, completed features, ops), [[concepts/actionable-signal]], [[concepts/speaker-attribution]], [[overview]], [[index]].

## [2026-07-01] wrapup | gor_dagster | Recursive LLM extraction — permanent doc `docs/architecture/features/recursive-llm-extraction.md` (interactive `si-gem31fl-recursive` / `fe-gem31fl-recursive`); deleted `docs/features/recursive-llm-extraction/`; Vertex AI batch delivery moved to `docs/features/batch-integration/` Phase 5; eval outputs → `docs/analytics/recursive-llm-extraction/`. Repo `WIKI.md` updated.

## [2026-07-01] sync | gor_dagster — Recursive LLM extraction wrapup: updated [[projects/gor_dagster]] (Stage 4 recursive SI, pipeline model priority, architecture decisions, active features, wrapped note), [[concepts/speaker-attribution]], [[concepts/llm-config-registry]], [[overview]], [[index]].

## [2026-07-07] wrapup | gor_dagster | Curation learning — permanent doc `docs/architecture/features/curation-learning.md`; P3 fund-noise, P2 unique-ticker bar 0.85, P1 Stage 0.75 promotion; sensor flags on; deleted `docs/features/curation-learning/`; `CLAUDE.md` + `WIKI.md` updated.

## [2026-07-07] sync | gor_dagster — Curation learning wrapup: new [[concepts/curation-learning]]; updated [[projects/gor_dagster]] Stage 6 cascade, architecture decisions, completed features; linked from [[concepts/resolution-pipeline-efficiency]]; [[index]] updated.

## [2026-07-10] wrapup | finfluencer-tracker | Landing page & conversion funnel improvements — permanent doc `docs/architecture/features/landing-conversion-improvements.md`; public `/leaderboard` + `/shows`; profile-depth free-account gate; unified landing teasers; SEO bot split; anon RPCs (`landing_stats`, show summary indexes); deleted `docs/features/landing-conversion-improvements/`. Repo `WIKI.md` updated.

## [2026-07-10] sync | finfluencer-tracker — Updated [[projects/finfluencer-tracker]] (access model, completed features, Supabase RPC notes, architecture decisions). Updated [[products/finfluencer-trade]] (component role, current status, cross-repo decision). Updated [[overview]], [[index]].

## [2026-07-11] wrapup | gor_dagster | CNBC IPO Scoreboard — permanent doc `docs/architecture/features/cnbc-ipo-scoreboard.md`; live `https://finfluencers.trade/cnbc-ipo` (SPCX v1); increments 7–8 social/blog deferred; deleted `docs/features/cnbc-ipo-scoreboard-social/`; removed throwaway diagnostic scripts. Repo `WIKI.md` updated.

## [2026-07-11] sync | gor_dagster — Updated [[projects/gor_dagster]] (Supabase IPO snapshot path, architecture decision, completed features, ops reference). Updated [[projects/finfluencer-tracker]] (`/cnbc-ipo` public route, RPC, decisions). Updated [[products/finfluencer-trade]], [[overview]], [[index]].

## [2026-07-13] sync | rattaproff — GSheet ground-truth sync safety: new repo doc `docs/architecture/features/gsheet-ground-truth-sync.md`; new [[concepts/gsheet-ground-truth-sync]]; updated [[projects/rattaproff]] (architecture decisions, completed features, current status), [[products/rattaproff]], [[overview]], repo `WIKI.md`, [[index]].

## [2026-07-15] sync | botastico — July SSL cert incident + monitoring: new [[projects/botastico]] (GCP LB certs, `ssl_cert_monitor`, uptime checks); new [[concepts/botastico-ssl-certificates]]; new [[decisions/botastico-gcp-managed-ssl-2026-07]]; updated [[products/botastico]], [[overview]], repo `WIKI.md`, [[index]].

## [2026-07-23] wrapup | gor_dagster | LinkedIn enrichment — permanent doc `docs/architecture/features/linkedin-enrichment.md`; trust-tiered discovery; published 100% trusted-or-none; no third-party API; deleted `docs/features/linkedin-enrichment/`; temp smoke/threshold scripts removed. Repo `WIKI.md` updated.

## [2026-07-23] sync | gor_dagster — LinkedIn enrichment wrapup: new [[concepts/linkedin-enrichment]]; updated [[projects/gor_dagster]] (Stage 6, Supabase sync, architecture decisions, completed features, ops); updated [[projects/finfluencer-tracker]] (profile LinkedIn, decisions); updated [[products/finfluencer-trade]], [[overview]], [[index]].

## [2026-07-27] sync | finfluencer-tracker — Subscription entitlement SSOT wrapup: new [[concepts/subscription-entitlement-ssot]]; updated [[projects/finfluencer-tracker]] (status, decisions, completed features, Supabase notes); updated [[products/finfluencer-trade]], [[overview]], [[index]].

## [2026-07-27] wrapup | gor_dagster | Gemini 3.5 Flash-Lite migration — permanent doc `docs/architecture/features/gemini-35-flash-lite-migration.md`; production SI `si-gem35fl-recursive` @450s / FE `fe-gem35fl-recursive` @1800s; ActionableSignal priority 1; dashboard Model Quality Comparison; deleted `docs/features/gemini-35-flash-lite-migration/`. Analytics promotion/rollback docs retained. Repo `WIKI.md` updated.

## [2026-07-27] sync | gor_dagster — Gemini 3.5 Flash-Lite wrapup: updated [[projects/gor_dagster]] (Stages 4a/5, ActionableSignal fe_priority, pipeline eligibility, dashboard, completed features); [[concepts/speaker-attribution]], [[concepts/actionable-signal]], [[concepts/llm-config-registry]]; [[products/finfluencer-trade]], [[overview]], [[index]].

## [2026-07-27] wrapup | gor_dagster | Investing Unscripted ingestion — permanent doc `docs/architecture/features/investing-unscripted-ingestion.md`; Pattern 6 twin; 272/272 downloaded; gor-blog Covered mapping; deleted `docs/features/investing-unscripted-ingestion/`. Repo `WIKI.md` updated.
## [2026-07-27] sync | gor_dagster — Investing Unscripted wrapup: updated [[projects/gor_dagster]] (10th RSS source, completed features, directory onboarding decision); [[concepts/onboarding-new-podcast-source]] (Req 13 / PR 5); [[projects/gor-blog]] directory status; [[overview]], [[index]].

## [2026-08-02] sync | gor_dagster — Chit Chat Stocks wrapup: updated [[projects/gor_dagster]] (11th RSS source, completed features, iTunes discovery + no mandatory idempotent re-launch); [[concepts/onboarding-new-podcast-source]]; [[projects/gor-blog]] directory deferred note; [[overview]], [[index]].

## [2026-08-04] ingest | Google Ads / GA4 conversion tracking topology (finfluencers.trade) — new [[concepts/google-ads-conversion-tracking]]. Verified end-to-end: ONE Google tag (`G-BLE3H05Q5T`, `GT-TWMLQVCG`, `AW-18322362149`, `GT-K8HQDHPS`) → destinations GA4 property `485294334` + Google Ads `AW-18322362149`; GA4 stream `10516700775` measurement ID **is** `G-BLE3H05Q5T`. Legacy tag display name "Finfluencers.Bet" (old domain) **renamed to "Finfluencers.Trade"** — root cause of repeated "two systems / missing tag" confusion. Corrected a false standing claim that the site had no `AW-` tag. GA4 property enabled as an Ads conversion data source (was never selected — the real reason no GA4 key event reached Ads); `view_page` imported as a conversion action. Morning-review scheduled task prompt rewritten with the ID-level topology. Updated [[index]].

## [2026-08-04] sync | finfluencer-tracker — Cumulative performance charts wrapup: new [[concepts/cumulative-performance-charts]]; updated [[projects/finfluencer-tracker]] (status, `/compare`, decisions, completed features, Supabase RPCs); updated [[products/finfluencer-trade]], [[projects/gor_dagster]] (SPY `benchmark_daily_prices` sync), [[concepts/signal-performance]], [[overview]], [[index]].

## [2026-08-04] ingest | Conversion signal correction (finfluencers.trade) — updated [[concepts/google-ads-conversion-tracking]]. Found the Google Ads PRIMARY conversion was "Registreerumine (Lehe laadimine finfluencers.trade/signup)", rule *"someone visits a page starting with finfluencers.trade/signup"* — it counted signup-FORM ARRIVALS, not registrations, so Smart Bidding optimised the wrong outcome and all historical cost/conv was measured against the wrong denominator. Swapped: native code-fired `sign_up` (AW-18322362149/6l99CO…, from `AuthCallback.tsx`) → **Esmane**; page-load action → **Teisene**. Also retired `view_page` the same day it was imported — a site-wide pageview key event collapsed GA4's key-events metric and created a primary-within-goal Ads action; un-keying it in GA4 broke the import as intended (confirmed mechanism). Audit of `finfluencer-tracker` showed the app already fires 11 GA4 events that were never marked key events — instrumentation was not the gap, wiring was. New spec `docs/features/conversion-measurement-plan/requirements.md`. Morning-review task prompt rewritten. Updated [[index]].

## [2026-08-11] sync | gor_dagster — Wrapups: signal source-quote restoration, LinkedIn outreach intro personalization, LinkedIn outreach performance content. New [[concepts/signal-source-quote]], [[concepts/linkedin-outreach]]; updated [[projects/gor_dagster]] (completed features, active folders, ops, GCS outreach path), [[projects/finfluencer-tracker]] (outreach render note), [[products/finfluencer-trade]], [[overview]], [[concepts/cumulative-performance-charts]], [[concepts/linkedin-enrichment]], [[index]].

## [2026-08-14] wrapup | gor-blog | Platform update blog + newsletter — post `cumulative-performance-and-show-leaderboards`; Kit broadcast 25439881; wrap-up `api/newsletter/cumulative-compare-launch.md`; deleted `features/platform-update-blog-newsletter/`.

## [2026-08-14] sync | gor-blog — Platform-update wrapup: updated [[projects/gor-blog]], [[concepts/blog-post-cta-pattern]], [[overview]], [[index]].

## [2026-08-14] sync | rattaproff — Category URL export from Woo REST slugs (not sheet-name transliteration). New repo doc `docs/architecture/category-url-export.md`; gitignored `category_urls_*.csv` in repo root. Updated [[projects/rattaproff]], [[products/rattaproff]] (20 storefronts, was stale 7), repo `WIKI.md`, [[overview]], [[index]].

## [2026-08-17] sync | finfluencer-tracker — Reddit pixel wrapup: new [[concepts/reddit-ads-conversion-tracking]]; updated [[projects/finfluencer-tracker]] (status, completed features, consent/SignUp decisions, CAPI deferred); [[products/finfluencer-trade]], [[concepts/google-ads-conversion-tracking]] (parallel channel), [[overview]], [[index]]. Permanent doc `docs/architecture/features/reddit-pixel-tracking.md`; `docs/features/reddit-pixel-tracking/` removed.

## [2026-08-17] sync | finfluencer-tracker — Public navigation discoverability wrapup: marketing Explore menu (Leaderboard / Compare / Shows) on header, hamburger, and footer; `EXPLORE_LINKS` SSOT; no `forceMount` on Radix panel. Updated [[projects/finfluencer-tracker]], [[products/finfluencer-trade]], [[overview]], [[index]]. Permanent doc `docs/architecture/features/public-navigation-discoverability.md`; `docs/features/public-navigation-discoverability/` removed.

## [2026-08-18] sync | finfluencer-tracker — Session replay wrapup: new [[concepts/session-replay-analytics]]; updated [[projects/finfluencer-tracker]], [[products/finfluencer-trade]], [[concepts/google-ads-conversion-tracking]], [[concepts/reddit-ads-conversion-tracking]], [[overview]], [[index]]. Permanent doc `docs/architecture/features/session-replay-analytics.md`; `docs/features/session-replay-analytics/` removed. Clarity live on Production; weekly review until 2026-09-17.

## [2026-08-19] sync | gor_dagster — Retired podcast-onboarding Requirement 9 (idempotency / safe re-runs). Playbook template is now Requirements 1–12 (directory = 12). Updated [[concepts/onboarding-new-podcast-source]], [[projects/gor_dagster]], [[projects/gor-blog]], [[index]].

## [2026-08-22] sync | finfluencer-tracker — Feedback decline with admin note wrapup: new [[concepts/feedback-roadmap]]; updated [[projects/finfluencer-tracker]] (status, completed features, notify-marker / vote-allowlist / orphaned-notify decisions), [[products/finfluencer-trade]], [[overview]], [[index]]. Permanent doc `docs/architecture/features/feedback-decline-with-note.md`; `docs/features/feedback-decline-with-note/` removed.

## [2026-08-22] sync | finfluencer-tracker — Mobile Core Web Vitals wrapup: new [[concepts/core-web-vitals-mobile]]; updated [[projects/finfluencer-tracker]] (status, completed features, lab-budget / eager-landing / idle-third-party / fetch-gated-header / field-data decisions), [[products/finfluencer-trade]], [[projects/gor-blog]] (four CrUX URLs are MkDocs), [[concepts/session-replay-analytics]] (idle injection), [[overview]], [[index]]. Permanent doc `docs/architecture/features/core-web-vitals-mobile.md`; `docs/features/core-web-vitals-mobile/` removed. Lab `/` LCP 2.03 s; Search Console field data ~2026-09-19.
