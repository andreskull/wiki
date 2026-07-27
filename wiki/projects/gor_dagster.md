---
type: project
title: "gor_dagster"
product: finfluencer-trade
project: gor_dagster
created: 2026-04-06
updated: 2026-07-27
tags: [dagster, pipeline, bigquery, gcs, python, stt, llm, speaker-attribution, facts-extraction, supabase, linkedin, gemini, podcast-rss]
---

# gor_dagster

The core backend of [[products/finfluencer-trade]]. A Dagster-orchestrated data pipeline that ingests financial influencer content (podcasts), transcribes it, attributes speakers, extracts financial predictions, and tracks signal performance over time.

## Product

[[products/finfluencer-trade]]

## Purpose and role

The data engine. Everything from RSS feed polling through to `ActionableSignal` creation runs here. The other finfluencer.trade repos (`gor-blog`, `finfluencer-tracker`) consume the structured data this pipeline produces. BigQuery is the single source of truth; Supabase is a read-replica synced via Dagster (`sync/sync_to_supabase` and related `mat_*` assets) for the app layer.

### Product and app planning (durable)

| Doc | Purpose |
|---|---|
| [MVP_MASTER_PLAN.md](file:///Users/andreskull/gor_dagster/docs/MVP_MASTER_PLAN.md) | Shipped **finfluencer-tracker** MVP — achievement spec |
| [INCR_01_MASTER_PLAN.md](file:///Users/andreskull/gor_dagster/docs/INCR_01_MASTER_PLAN.md) | Deferred / follow-on product work after MVP |
| [finfluencers-app-runbook.md](file:///Users/andreskull/gor_dagster/docs/operations/finfluencers-app-runbook.md) | App operations, access rules, E2E, cutover checks |

Growth and GTM live in [`gor-blog/growth_plan.md`](file:///Users/andreskull/gor-blog/growth_plan.md). **Do not** use `docs/features/<feature>/` as the long-term home for planning — those folders are temporary until `/wrapup`; see **Active features in progress** below. Hub page: [[products/finfluencer-trade]] § Planning and strategy.

---

## Tech stack

| Layer | Technology |
|---|---|
| Orchestration | Dagster 1.10.19 — Cloud hybrid, GCE agent |
| Language | Python ≥ 3.10 |
| Dependency management | Poetry — all Dagster packages pinned to exact versions |
| Data warehouse | Google BigQuery — **`dagster_prod`** (RSS/content catalogue) + **`dagster_shared`** (pipeline warehouse) + **`dagster_prices`** (prices and signal performance); see [[entities/bigquery]] |
| App database | Supabase (PostgreSQL) — nightly read-replica of BigQuery |
| File storage | GCS: `gor-media-prod` (audio/images), `gor-stt-transcripts` (transcripts — hardcoded) |
| STT providers | AssemblyAI, Deepgram Nova-2/Nova-3, ElevenLabs Scribe, Google STT, Speechmatics, Rev.AI |
| LLM providers | OpenAI (GPT-5, GPT-4o, O3-mini), Google Gemini (2.5-flash, 2.0-flash, 2.5-pro), Anthropic Claude, xAI Grok |
| Key libraries | pydantic, instructor, pandas, streamlit, plotly, tiktoken, tenacity, feedparser |
| CI/CD | GitHub Actions → Docker → GCE VM → Dagster Cloud |

Dagster packages must all be upgraded together — version mismatch between `dagster`, `dagster-cloud`, `dagster-gcp`, `dagster-postgres` causes runtime failures.

---

## Pipeline stages

The pipeline is a seven-stage sequence. Each stage consumes the output of the previous.

### Stage 1 — RSS Ingestion

Polls RSS feeds for all configured `ContentSource` records. For each new episode: creates a `ContentItem`, downloads audio to `gs://gor-media-prod/sources/podcasts/{show_id}/{episode_id}.mp3`. The `rss_episode_ingestion_sensor` runs on a configurable `RSS_POLL_INTERVAL_SEC` (default 300s). Download retries use exponential backoff; permanent failures (`download_error:unsupported_media_type`, `download_error:network`) stop retries and flag for manual review.

**Production RSS catalogue (ten `podcast_rss` ContentSources; SI via `get_podcast_rss_content_source_ids()`):** Fast Money (`49400b5b-6e3e-4c0d-be0b-8cd7ab18ba74`), Mad Money (`e6a22166-82ca-482d-b54b-4a1f016948c3`), Hedgeye (`8a61349e-9df2-4397-bc76-b4af8b0fb9d8`), Halftime Report (`c925324b-63e2-4f10-a074-6e4ed7da2d0b`), Morning Filter (`4f158ea6-c1c4-43f1-86e3-3d96fed6dd80` — [morning-filter-ingestion.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/morning-filter-ingestion.md)), **Compound and Friends** (`0b75ea6c-20b2-4a7c-89db-b0899788a8cc` — [compound-and-friends-ingestion.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/compound-and-friends-ingestion.md)), **7investing** (`c2658090-942b-4cbd-9552-f04995220873` — [7investing-ingestion.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/7investing-ingestion.md), Anchor/Spotify feed `https://anchor.fm/s/1659b6fc/podcast/rss`, `external_id` `1659b6fc`, enclosure **Pattern 4** Megaphone `APO…` and **Pattern 5** Anchor `anchor.fm/s/...`), **The Acquirers Podcast** (`428270b1-e53b-494a-b1b5-4278756c6aa4` — [acquirers-podcast-ingestion.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/acquirers-podcast-ingestion.md), Anchor/Spotify feed `https://anchor.fm/s/9603714/podcast/rss`, `external_id` `9603714`, Patterns 4+5, mid-catalog `.m4a` supported), **Motley Fool Hidden Gems Investing** (`22e0168c-ef5b-4864-a2ed-cbe8aff56d15` — [hidden-gems-ingestion.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/hidden-gems-ingestion.md), Megaphone direct RSS `feeds.megaphone.fm/ARML8165884693`, `external_id` `ARML8165884693`, enclosure **Pattern 6** Megaphone `ARML…`), and **Investing Unscripted** (`ff6a3fbe-590d-4693-88f3-d8b17be98700` — [investing-unscripted-ingestion.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/investing-unscripted-ingestion.md), Megaphone direct RSS `feeds.megaphone.fm/ARML8770731717`, `external_id` `ARML8770731717`, Pattern 6 twin, **272/272** downloaded **2026-07-27**; gor-blog directory Covered `investing-unscripted`). Episode ID extraction: Patterns 1–6 in [`rss_ingestion.py`](file:///Users/andreskull/gor_dagster/gor_dagster/assets/rss_ingestion.py). SI discovery: [`get_podcast_rss_content_source_ids`](file:///Users/andreskull/gor_dagster/gor_dagster/sensors/si_sensor.py) (all active `podcast_rss` sources — no hardcoded allowlist since **2026-07-01**).

Key assets: `rss_new_episodes_identified`, `rss_episode_ingestion_orchestrator`, `rss_episode_ingestion_sensor`

Docs: [RSS Ingestion Architecture](file:///Users/andreskull/gor_dagster/docs/architecture/rss-ingestion-architecture.md), [RSS Ingestion Guide](file:///Users/andreskull/gor_dagster/docs/operations/rss-ingestion-guide.md), [Onboarding a New Podcast Source — Playbook](file:///Users/andreskull/gor_dagster/docs/operations/onboarding-new-podcast-source.md) (wiki: [[concepts/onboarding-new-podcast-source]])

### Stage 2 — STT Transcription

Batch-processes audio against one or more STT providers. The `stt_batch_processor` asset handles large backlogs with quota management, provider-agnostic configuration, and sensor-based triggering. Raw outputs written to `gs://gor-stt-transcripts/raw/{episode_id}/{provider}_{model}.json`.

Six providers are supported. Provider registry pattern allows adding new providers without changing pipeline logic. Each provider has centralised pricing so cost estimation is automatic.

Key assets: `stt_batch_processor`, `stt_evaluation` assets, `targeted_transcription`

Docs: [STT Batch Processing Architecture](file:///Users/andreskull/gor_dagster/docs/architecture/stt-batch-processing-architecture.md), [STT Provider Configuration](file:///Users/andreskull/gor_dagster/docs/architecture/stt-provider-configuration.md)

### Stage 3 — Unified Transcript

The `stt_unified_converter` normalises provider-specific raw transcripts into a standard format. Written to `gs://gor-stt-transcripts/unified/{episode_id}/{provider}_{model}.json`. All downstream stages consume only unified format — they are provider-agnostic.

Coverage metrics are tracked: `stt_coverage_ratio` (target > 0.95) flags transcripts where the last utterance ends significantly before the expected audio duration. Diagnostics stored in `gs://gor-stt-transcripts/stt_diagnostics/{episode_id}/{stt_provider}.json`.

**ElevenLabs mono-speaker resplit (2026-07-01):** When ElevenLabs mono-diarization collapses an episode into mega-utterances, `unified_transcript_normalizers.py` splits utterances into **120s windows** (trigger: any utterance >480s or mono-speaker share ≥85%). **`elevenlabs_unified_heal.py`** can re-unify from the existing GCS raw blob at SI load (no STT re-call; max 2 attempts). Manual bulk re-unify: `scripts/rerun_unified_elevenlabs.py`. Permanent doc: [hidden-gems-ingestion.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/hidden-gems-ingestion.md) §ElevenLabs mono-speaker SI hardening.

Docs: [Unified Transcript Schema](file:///Users/andreskull/gor_dagster/docs/schemas/unified-transcript-schema.md)

### Stage 4 — Speaker Attribution

Two-stage pipeline: **LLM Identification** then **Transcript Hydration**.

**LLM Identification (Stage 4a):** Production uses **memory-centric recursive SI** via **`si-gem35fl-recursive`** (@**450s** windows; model promote **2026-07-27** — [gemini-35-flash-lite-migration.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/gemini-35-flash-lite-migration.md); algorithm [recursive-llm-extraction.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/recursive-llm-extraction.md)): three-wave algorithm (roster bootstrap → parallel window passes → optional resolution → deterministic fold). In-pass fallback: gem35fl → gem31fl → dsv4fr-recursive. Prior **`si-gem31fl-recursive`** artefacts stay readable via source-priority. Legacy single-pass configs remain as compat tails (`si-dsv4fr-58k`, grok-era ids).

An LLM classifies each utterance's speaker at one of three confidence levels (`full_name`, `partial_name`, `unknown`). The output — a flat segment array with `source_utterance_start/end` references — is written to `gs://gor-stt-transcripts/identification/{episode_id}/{provider}_{llm_config_id}.json`. Eight+ LLM model families supported via config registry. Anchor enforcement ensures `full_name` attributions require explicit textual evidence (self-introduction, name mention, etc.); speakers without anchors are forced to `Unknown`.

**Pending:** Vertex AI **batch delivery** for recursive SI backlog cohorts (~50% interactive cost) — active spec `gor_dagster/docs/features/batch-integration/` Phase 5; types stubbed in `EpisodeWaveState` / `WavePhase`.

**Transcript Hydration (Stage 4b):** Merges the identification file back into the unified transcript to produce a stitched transcript where every utterance has a named speaker. Written to `gs://gor-stt-transcripts/hydrated/{episode_id}/{provider}_{llm_config_id}.json`. The `hydration_sensor` builds batches of `(episode, llm_config_id)` pairs (**batch size 30**), emits **one** `hydration_job` run per sensor tick (fewer Dagster Cloud orchestration runs than the former dual parallel pattern), with round-robin fairness. Audit trail in the `hydration_runs` BigQuery table.

**In-memory algorithm (2026-06-15):** Canonical implementation in `gor_dagster/utils/transcript_hydration_utils.py` (asset re-exports). Hot path is **O(U log W + W)** via `WordIndex` + normalization cache (was O(U·W) full word scans — ~11 min → ~0.09s on large fixture). Production uses **FR-6** start-anchored word membership (`MEMBERSHIP_TOL=0.01`); recovers boundary/short utterances and speaker-disagreement words without changing golden speaker accuracy. Permanent doc: [hydration-performance-optimization.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/hydration-performance-optimization.md); algorithm reference: [transcript-hydration-architecture.md](file:///Users/andreskull/gor_dagster/docs/architecture/transcript-hydration-architecture.md) §In-memory hydration algorithm.

Quality metrics tracked per run: `anchor_coverage_pct` (target ≥ 95%), `llm_coverage_ratio` (target > 0.95), `multi_speaker_utterances_count`, `auto_corrected_segments_count`, `quote_validation_failure_rate`.

**Duration coverage gate (70% minimum):** `llm_coverage_calculator.calculate_unique_time_covered()` uses `merge_metadata.merged_utterances` when segments were merged across STT utterances — without this, merged segments only expose the first utterance bounds and under-report coverage (2026-07-01 fix). See [core-data-model.md](file:///Users/andreskull/gor_dagster/docs/architecture/core-data-model.md) (LLM Coverage Metrics).

**ElevenLabs auto-heal at SI load (2026-07-01):** `speaker_attribution_llm` calls `heal_elevenlabs_unified_transcript_if_needed()` before recursive SI; `si_sensor` excludes healable mega-utterance failures from `si_stuck`. See [hidden-gems-ingestion.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/hidden-gems-ingestion.md) §ElevenLabs mono-speaker SI hardening.

Key assets: `speaker_attribution_llm`, `transcript_hydration`, `statistical_speaker_attribution`

Docs: [Speaker Attribution Architecture](file:///Users/andreskull/gor_dagster/docs/architecture/speaker-attribution-architecture.md), [Recursive LLM extraction](file:///Users/andreskull/gor_dagster/docs/architecture/features/recursive-llm-extraction.md), [Anchor Enforcement](file:///Users/andreskull/gor_dagster/docs/architecture/speaker-attribution-anchor-enforcement.md), [Speaker Attribution Guide](file:///Users/andreskull/gor_dagster/docs/operations/speaker-attribution-guide.md), [Transcript Hydration Architecture](file:///Users/andreskull/gor_dagster/docs/architecture/transcript-hydration-architecture.md), [Hydration performance optimization](file:///Users/andreskull/gor_dagster/docs/architecture/features/hydration-performance-optimization.md), [Hydration sensor batching](file:///Users/andreskull/gor_dagster/docs/operations/hydration-sensor-batching.md)

### Stage 5 — Facts Extraction

An LLM reads the hydrated transcript and extracts financial predictions: stock picks, position disclosures (start/hold/close long/short), recommendations. Each extracted item becomes a `PotentialPrediction` row in BigQuery. Runs in parallel with `IndividualQuote` creation from the same hydrated transcript.

FE model selection: production **`fe-gem35fl-recursive`** (@**1800s** windows; paired with recursive SI; promote **2026-07-27**). Job-level retry second slot remains **`fe-dsv4fr-58k`**. Chooses the best available hydration (per `Pipeline Model Priority and Retries` policy). Retries upward only — if a lower-priority hydration succeeds first, FE runs on it, then automatically re-runs on a better hydration when available.

`PotentialPrediction` is the raw store. It holds everything including duplicates (from running with different FE configs). `ActionableSignal` is a VIEW over it with deduplication logic.

Key assets: `facts_extraction`, `individual_quote_creation`

Docs: [Facts Extraction Architecture](file:///Users/andreskull/gor_dagster/docs/architecture/facts-extraction-architecture.md)

### Stage 6 — Instrument + Speaker Resolution

**Job:** `potential_prediction_resolution_job` → asset `resolve_pending_predictions` (speakers first, then instruments; `in_process_executor`). Sensor: `potential_prediction_resolution_sensor` (2h cadence).

**Instrument Resolution:** Maps raw ticker/company mentions to canonical `FinancialInstrument` entities via layered matching:

- Step 0: `DismissedInstrumentHash` auto-dismiss
- Layer 0.5: `InstrumentAlias` instant lookup
- **Layer 0.75: promoted-ticker lookup** (≥2 consistent manual curations, no conflicting aliases — **2026-07-07**)
- Layers 1/2: internal JW fuzzy match (`compute_instrument_name_similarity`); **unique-ticker bar 0.85** when ticker maps to exactly one FI (not inferred)
- Layer 1.5: historical ticker lookup
- Layer 3: OpenFIGI batch lookup

**Curation learning (2026-07-07):** P3 fund-noise second pass in name similarity (always on). P1+P2 flag-gated in production via `potential_prediction_resolution_sensor`. Poison-alias cleanup manifest + gap-fix scripts applied in prod BQ. Permanent doc: [curation-learning.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/curation-learning.md). Wiki: [[concepts/curation-learning]].

Unresolvable instruments queue in `PendingInstrumentResolution`. **Re-attempt machinery (2026-06):** frozen rows re-enter when `reattempt_eligible=TRUE` (sibling ticker mark or backlog sweep). Resolver version `instr-2026.07-tr`. Alias written on every successful auto-resolve (`created_by='auto_resolution'`).

**Dashboard enrichment (2026-06-30):** Instrument curation matches existing `FinancialInstrument` rows **FIGI-first**, with unambiguous `(ticker, exchange)` fallback when OpenFIGI returns a different FIGI than stored — prevents false **"Create & link"** for well-known names like SPY. Ambiguous recycled tickers are never auto-linked.

**SPY canonical identity (2026-06-30):** Benchmark ETF SPY/US uses single Bloomberg FIGI **`BBG000BDTBL9`** (`gor_dagster/configs/benchmark_figi.py`). Row UUID **`8dbef73e-8ec3-4b18-b530-b045316467f1`** unchanged; daily price sync and benchmark SQL follow the FI row's FIGI. Daily **`spy_single_identity`** asset check on `resolve_instruments` + `scripts/verify_spy_single_identity.py`; schedule STOPPED by default. Permanent doc: [spy-canonical-figi-consolidation.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/spy-canonical-figi-consolidation.md); runbook: [spy-figi-consolidation-runbook.md](file:///Users/andreskull/gor_dagster/docs/operations/spy-figi-consolidation-runbook.md).

**Speaker Resolution:** Maps named speakers to `Finfluencer` via `SpeakerMatchScorer` (Jaro-Winkler + org context). Production thresholds: fuzzy auto-resolve **0.935**; org-conflict override **disabled** (1.00). Unresolved speakers queue in `PendingSpeakerResolution`.

**LinkedIn enrichment (2026-07-23):** On auto-create (when `SPEAKER_ENABLE_LINKEDIN_DISCOVERY` is on), resolution may run Gemini+search discovery (`linkedin-gemini-flash`) and queue corroborated candidates — **never** auto-write profiles. Trusted LinkedIn URLs (Notion seed, curator Accept/Set URL, speaker-create XOR URL/unavailable) sync to Supabase `finfluencers.linkedin_url`. Published cohort **100% trusted-or-none**. No third-party LinkedIn API. Permanent doc: [linkedin-enrichment.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/linkedin-enrichment.md). Wiki: [[concepts/linkedin-enrichment]].

**Proof-segment speaker resolution (2026-07-01):** After prediction-level resolution, `_resolve_proof_segment_speakers` resolves each distinct speaker in `proof_segments[]`. Multi-token unresolved secondaries appear in the **unified dashboard queue** (ranked by `predictions_blocked`); `resolve_proof_segment_speaker()` writes `finfluencer_id` back to all affected predictions. `content_quality.proof_segment_speaker_backfill` propagates manual resolutions historically. `display_name` for quotes is enriched at `mat_signals` build (`actionable_signal_sql`), not stored in BQ. Permanent doc: [proof-segment-speaker-resolution.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/proof-segment-speaker-resolution.md). Wiki: [[concepts/proof-segment-speaker-resolution]].

**Backlog sweep asset:** `resolution_backlog_sweep` — dry-run preview then mark eligible; drain with resolution job. One-time sweeps unlocked **237** instrument + **18** speaker hashes (2026-06-09).

**Ops monitoring:** `scripts/analyze_resolution_backlog.py`, `scripts/analyze_resolution_drill.py`.

Permanent reference: [resolution-pipeline-efficiency.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/resolution-pipeline-efficiency.md). Wiki: [[concepts/resolution-pipeline-efficiency]].

Docs: [Instrument Resolution Reference](file:///Users/andreskull/gor_dagster/docs/architecture/instrument-resolution-reference.md), [Speaker Resolution Workflow](file:///Users/andreskull/gor_dagster/docs/operations/speaker-resolution-workflow.md), [Speaker Resolution Human Priors](file:///Users/andreskull/gor_dagster/docs/architecture/speaker-resolution-human-priors.md), [Instrument resolution bulk curation](file:///Users/andreskull/gor_dagster/docs/architecture/features/instrument-resolution-bulk-manual-curation.md)

### Stage 7 — Signal Generation and Performance Tracking

`ActionableSignal` (a BigQuery VIEW) surfaces only the `PotentialPrediction` rows that meet all gates:
- `speaker_resolution_status = 'resolved'`
- `resolution_status = 'resolved'` (instrument resolved)
- `extraction_confidence >= 0.7`
- `finfluencer.status = 'active'`
- `proof_segments_speaker_status = 'resolved'`
- FE config priority deduplication (highest-priority FE config per episode wins — DeepSeek `fe-dsv4fr-*` above retained grok tiers when both exist; see `ActionableSignal.sql`)

`SignalPerformance` tracks each signal across standard horizons (1w, 1m, 3m, 6m, 1y). Price data ingested via EODHD API. **S&P 500 benchmark** uses SPY at **`BBG000BDTBL9`** (see SPY consolidation above). **Exit pricing (2026-06-30):** horizon-end prices use **as-of** join — latest `PriceHistory` bar on or before the calendar end date when exact-day bars are missing (avoids NULL benchmark rows on sparse trading days). **Truncated horizons** (position ends before the horizon completes) are **not** stored. A position ends on an explicit `close_long` / `close_short`, or **implicitly** when the same finfluencer issues an opposite-direction `start_*` / `hold_*` on the **same instrument** (boundary = `first_tradeable_session_date` of the flip). **Signal pruning (2026-05):** within-sequence redundant mentions are pruned in `SignalSequence`; Supabase **`mat_signal_performance`** merges un-pruned **`SignalPerformance`** with `mat_signals`, while finfluencer aggregate mats filter with **`mat_signals.is_kept`** so leaderboards reflect kept calls. **`SignalPerformance_pruned`** is the kept-only calculator table for QA. The cutover snapshot **`SignalPerformance_baseline`** is retired — Dagster does not write it; the physical BigQuery table was dropped after cutover ([feature doc](file:///Users/andreskull/gor_dagster/docs/architecture/features/signal-pruning-performance.md)). See [[concepts/signal-performance]] and [Performance Methodology](file:///Users/andreskull/gor_dagster/docs/architecture/performance-methodology.md).

`SignalCurrentPerformance` (BigQuery VIEW) and the `calculate_signal_performance` asset share the same truncation logic (`implicit_close_signals` ∪ `close_signals` in SQL).

Signal date uses `ContentSourceTimingConfig` per show — `pubDate` from RSS is not reliable for air date, especially for shows like Mad Money that publish after market close.

Docs: [ActionableSignal Schema](file:///Users/andreskull/gor_dagster/docs/schemas/actionable-signal-schema.md), [Performance Methodology](file:///Users/andreskull/gor_dagster/docs/architecture/performance-methodology.md), [Signal Date vs Publication Date](file:///Users/andreskull/gor_dagster/docs/operations/signal-date-vs-publication-date.md), [Signal Timing and Source Config](file:///Users/andreskull/gor_dagster/docs/operations/signal-timing-and-source-config.md)

---

## Data model

### BigQuery datasets (`gurus-on-record`)

Production uses **three** datasets for the core pipeline. Default `bigquery_resource` targets `BIGQUERY_DATASET_ID` (typically `dagster_prod` per `env.example`); `app_bq_resource` is fixed to `dagster_shared` in `definitions.py`; price/performance objects live in `dagster_prices`.

#### Content catalogue — `dagster_prod`

| Entity | What it is |
|---|---|
| `ContentSource` | A podcast feed (RSS URL, metadata, image) |
| `ContentItem` | One episode from a source |
| `ContentSourceTimingConfig` | Per-show rules for signal date vs publication date |

#### Pipeline warehouse — `dagster_shared`

| Entity | What it is |
|---|---|
| `ContentSegment` | A time-bounded chunk of an episode (ad, main content) — in repo schemas; confirm table exists in your project before querying |
| `ContentMedia` | GCS path to audio/image/video associated with an item or segment — in repo schemas; confirm deployment |
| `Finfluencer` | A canonical financial influencer entity |
| `FinfluencerNameVariant` | All known name variants for a Finfluencer (used for resolution) |
| `FinfluencerBioHistory` | Historical bios with effective dates |
| `FinfluencerAffiliation` | Organisation + role, used for resolution boosting |
| `FinfluencerPlatformProfile` | Social/platform profiles per Finfluencer (`provenance_json` for LinkedIn trust tiers) |
| `PendingLinkedInResolution` | Uncertain LinkedIn candidates for curator review |
| `LinkedInLookupAttempt` | Append-only LinkedIn discovery cost/outcome log |
| `LinkedInCoverage` | **VIEW** — coverage KPIs by segment (signal-bearing / long-tail) |
| `ContentItemContributor` | Links an episode to a Finfluencer (host, guest, author) — schema may exist; LinkedIn show names use PP→ContentItem→ContentSource |
| `IndividualQuote` | One utterance from the hydrated transcript, attributed to a speaker |
| `FinancialInstrument` | Canonical stock/instrument (FIGI, ticker, exchange, sector) |
| `PotentialPrediction` | Raw LLM extraction — all hypotheses including duplicates across FE configs |
| `ActionableSignal` | **VIEW** over `PotentialPrediction` — deduped, gated, production-ready |
| `PredictionContext` | Audio/video clip context for a signal |
| `PendingSpeakerResolution` | Speaker names that need human review to map to a Finfluencer |

#### Price and performance warehouse — `dagster_prices`

| Entity | What it is |
|---|---|
| `PriceHistory` | Daily OHLCV and adjusted prices used for returns |
| `TradingCalendar` | Trading-session calendar and sequential day numbers |
| `SignalPerformance` | Measured performance per signal per time horizon (full calculator output; source for `mat_signal_performance`) |
| `SignalPerformance_pruned` | Kept-only performance rows (`calculate_pruned_signal_performance`) for pruning QA |
| `SignalPerformance_baseline` | **Removed** — cutover-only snapshot; not recreated by Dagster; physical table dropped **2026-05** |

**Operational tables (`dagster_shared`):**

| Table | What it tracks |
|---|---|
| `stt_operations` | All STT operations: provider, cost, duration, coverage ratio |
| `stt_speaker_attributions` | Speaker identification runs — quality metrics per episode × provider × LLM |
| `hydration_runs` | Transcript hydration audit trail |
| `facts_extraction_runs` | FE asset telemetry |
| `llm_weights` | F1 scores and composite weights per episode × LLM config |
| `stt_keyword_evaluations` | Provider keyword capture quality metrics |

### The ActionableSignal VIEW — critical rules

- **Never DELETE from `ActionableSignal`** — it's a VIEW. Delete from `PotentialPrediction` instead.
- **FE config priority deduplication** — only the highest-priority `fe_config_id` per episode appears. Priority order (**`gor_dagster/sql/views/ActionableSignal.sql` `fe_priority`**, promote **2026-07-27**): **`fe-gem35fl-recursive`** (1) → **`fe-gem31fl-recursive`** (2) → **`fe-gpt-5.2`** → **`fe-gpt-5`** → **`fe-dsv4fr-*`** → **`fe-grok-4-fast-reasoning*`** → other / unknown. **`scripts/update_actionable_signal_view.py`** deploys edits to `fe_priority`.
- **Proof-segment speaker gate** — every row requires `proof_segments_speaker_status = 'resolved'`. No created_at cutoff. `display_name` on proof segments resolves at `mat_signals` build from `FinfluencerNameVariant`, not stored in `PotentialPrediction`. **73,261** resolved / **42** long-tail partial-pending as of **2026-07-01**. [[concepts/proof-segment-speaker-resolution]]

### GCS storage structure

```
gs://gor-stt-transcripts/           ← hardcoded in definitions.py (line 206)
├── raw/{episode_id}/{provider}.json
├── unified/{episode_id}/{provider}.json
├── identification/{episode_id}/{provider}_{llm_config_id}.json
├── hydrated/{episode_id}/{provider}_{llm_config_id}.json
├── statistical_attribution/{episode_id}_...
├── stt_diagnostics/{episode_id}/{provider}.json
└── speaker_attribution_diagnostics/{episode_id}/{provider}/{llm_config_id}.json

gs://gor-media-prod/                ← from GCS_BUCKET_NAME env var
├── sources/podcasts/{show_id}/{episode_id}.mp3
└── images/{entity_id}/{filename}
```

---

## Key subsystems

### LLM configuration registry

A code-based registry (`gor_dagster/resources/llm_provider_registry.py`, `llm_resource_factory.py`) that manages all LLM experimentation. Config IDs like `gemini`, `gpt5`, `gpt4o`, `claude4_sonnet` reference specific model + parameters + prompt template combinations. Every LLM result file in GCS is named with its `llm_config_id` — results are never overwritten across configs. Tracks cost (USD), token usage, and latency per run.

The registry is general-purpose — used for speaker attribution (SI), transcript hydration (HY), facts extraction (FE), and any future LLM task. Each use case (SI / HY / FE) has its own config prefix and GCS path convention.

Docs: [LLM Configuration Architecture](file:///Users/andreskull/gor_dagster/docs/architecture/llm-configuration-architecture.md), [LLM Developer Guide](file:///Users/andreskull/gor_dagster/docs/architecture/llm-developer-guide.md)

### Dynamic token allocation

Token limits per LLM call are set dynamically based on episode length, segment count, and a configurable `token_ceiling`. The `DynamicTokenAllocator` computes per-call limits to avoid both truncation (too low) and wasted cost (too high). Token ceiling experiments in the analytics docs validated optimal settings.

Docs: [Dynamic Allocation API](file:///Users/andreskull/gor_dagster/docs/api/dynamic-allocation.md)

### Pipeline model priority and retries

Central policy in **`gor_dagster/configs/pipeline_eligibility.py`**. Production SI/FE (**2026-07-27**): **`si-gem35fl-recursive`** (@450s) / **`fe-gem35fl-recursive`** (@1800s); in-pass fallback gem35fl → gem31fl → dsv4fr-recursive; gem31fl + dsv4fr/grok/gpt remain source-priority / ActionableSignal-readable. Keep **four ladder surfaces distinct** (in-pass ≠ SI/FE job retry ≠ source-priority). Narrative: **[gemini-35-flash-lite-migration.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/gemini-35-flash-lite-migration.md)**, **[recursive-llm-extraction.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/recursive-llm-extraction.md)**, **[pipeline-model-priority-and-retries.md](file:///Users/andreskull/gor_dagster/docs/architecture/pipeline-model-priority-and-retries.md)**. Governs:
- Which LLM configs are eligible for SI (Speaker Identification), HY (Hydration), FE (Facts Extraction)
- Duration-based SI starting config (short episodes vs long)
- Upward-only retries — FE always picks the best available hydration, re-runs automatically on improvement
- Dashboard alignment — same configs shown in monitoring as are used in production

Docs: [Pipeline Model Priority and Retries](file:///Users/andreskull/gor_dagster/docs/architecture/pipeline-model-priority-and-retries.md)

### Golden reference system

Creates verified ground truth transcripts for evaluating STT + LLM combinations. Human reviewers correct only the flagged deviations (statistical consensus reduces review burden ~90%). Golden references are versioned: `golden_v1.json` → `golden_v2.json` etc. Latest version is always the source of truth. Used for benchmarking which STT + LLM config combination gives the best results.

Docs: [Golden Reference Architecture](file:///Users/andreskull/gor_dagster/docs/architecture/golden-reference-architecture.md), [Golden Reference Workflow](file:///Users/andreskull/gor_dagster/docs/operations/golden-reference-workflow.md)

### Finfluencer pre-seeding (master data)

A clustering engine that resolves the "cold start" problem — populating the `Finfluencer` table before manual curation. Uses name clustering, affiliation context, and platform profile data to create provisional Finfluencer records (`status = 'pending_confirmation'`). Human curation promotes them to `active` or marks as `merged`/`rejected`.

Docs: [Finfluencer Master Data](file:///Users/andreskull/gor_dagster/docs/architecture/finfluencer-master-data.md)

### Supabase sync layer

BigQuery is the system of record. Supabase mirrors selected tables for the app (`finfluencer-tracker`). Dagster asset `sync/sync_to_supabase` (and `mat_*` materialisation assets) materialise compatible tables, upsert deltas, and **delete stale rows** when BigQuery no longer has a matching key (e.g. after performance row corrections).

**CNBC IPO scoreboard (2026-07-11):** Dedicated snapshot path for post-IPO CNBC cohort picks — `mat_ipo_scoreboard_{signals,leaderboard,summary}` in `dagster_shared` → Supabase `ipo_scoreboard_*` tables → RPC `get_ipo_scoreboard_page`. Since-call performance via `IpoScoreboardSinceCallPerformance` (not fixed horizons). Public app route **`/cnbc-ipo`** on [[projects/finfluencer-tracker]]; v1 ticker **SPCX** only. UI shows **kept picks only** (`is_kept`); repeat mentions hidden from feed. Social honeypot + gor-blog launch **deferred** (sparse post-IPO pick frequency). Permanent doc: [cnbc-ipo-scoreboard.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/cnbc-ipo-scoreboard.md). Ops: `scripts/verify_ipo_scoreboard_rpc_prod.py`, `scripts/analyze_hot_ipo_scoreboard_candidates.py`.

**LinkedIn URL sync (2026-07-23):** `finfluencers.linkedin_url` receives only **trusted** provenance (Notion seed + curator / speaker-create writes). Provisional discovery rows stay in BQ. Ops: `scripts/verify_linkedin_supabase_sync.py`. [[concepts/linkedin-enrichment]]

The app does not write to synced analytical tables — signal and performance data flow BigQuery → Supabase.

Docs: [Supabase Schema Spec](file:///Users/andreskull/gor_dagster/docs/architecture/supabase-schema-spec.md), [Supabase Sync Architecture](file:///Users/andreskull/gor_dagster/docs/architecture/supabase-sync-architecture.md)

### Pipeline monitoring dashboard

A Dash/Plotly **pipeline dashboard** (`pipeline_dashboard/`) with real-time metrics. Sections: STT/SI/FE process monitoring, recursive SI monitoring, **Model Quality Comparison** (SI/FE proxies by `llm_config_id` + model family — **2026-07-27**), LinkedIn/instrument curation, costs, errors. Family map: `pipeline_dashboard/config_registry.py` `config_id_to_family`.

Docs: [Pipeline Dashboard Architecture](file:///Users/andreskull/gor_dagster/docs/architecture/pipeline-dashboard-architecture.md), [gemini-35-flash-lite-migration.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/gemini-35-flash-lite-migration.md)

---

## Deployment and infrastructure

- **Hybrid Dagster Cloud** — Dagster Cloud manages orchestration; a GCE VM (`AGENT_VM_MACHINE_TYPE_PROD`, e.g. `e2-standard-4`) runs the agent in Docker
- **Separate staging VM** — `AGENT_VM_MACHINE_TYPE_STAGING` (e.g. `e2-medium: 2 vCPU, 4GB`) for branch deployments; resource constrained, avoid heavy parallelism
- **Default executor: `max_concurrent: 2`** — prevents GCE VM contention. Jobs that need different behaviour override explicitly (e.g. `instrument_resolution_job` uses `in_process_executor`)
- **Dagster Cloud orchestration credits** — hybrid deployment bills credits for orchestrated runs; cadence and run fan-out are tuned per [Dagster Cloud credit optimization](file:///Users/andreskull/gor_dagster/docs/architecture/features/dagster-cloud-credit-optimization.md) (e.g. Supabase sync **N=8** in production via `SUPABASE_SYNC_INTERVAL_HOURS_PROD`, PP resolution sensor **2h** minimum interval)
- **`in_process_executor` for DNS-sensitive jobs** — multiprocess child workers can't resolve `gor.agent.dagster.cloud`; use `in_process_executor` as workaround
- **Deploy flow**: `git push origin main` → GitHub Actions builds Docker image → pushes to GCR → GCE agent picks up new image
- **Environments**: `local` (filesystem I/O), `staging` (branch deploys), `production` (main branch). BigQuery still uses the same two production datasets (`dagster_prod` + `dagster_shared`); branch deployments do not create a separate staging copy of the warehouse — see [[entities/bigquery]]

Docs: [Deployment Strategy](file:///Users/andreskull/gor_dagster/docs/architecture/deployment-strategy.md), [Infrastructure](file:///Users/andreskull/gor_dagster/docs/architecture/infrastructure.md), [Agent VM Machine Type Split](file:///Users/andreskull/gor_dagster/docs/operations/agent-vm-machine-type-split.md)

---

## Code structure

```
gor_dagster/
├── gor_dagster/
│   ├── assets/           ← @asset definitions (14 files, flat — NO subfolders)
│   ├── services/         ← standalone business logic (BigQueryLLMWriter)
│   ├── integrations/     ← adapters between systems (TokenMetrics ↔ BigQuery)
│   ├── models/           ← Pydantic models (13 files: token, speaker, STT, transcript)
│   ├── resources/        ← Dagster resource definitions (GCP, LLM, STT)
│   ├── configs/          ← pipeline_eligibility.py, inline_f1_config.py
│   ├── jobs/             ← job definitions
│   ├── prompts/          ← LLM prompt templates
│   ├── sql/              ← schemas/ and views/
│   └── utils/            ← 25+ utility modules
├── configs/              ← version-controlled YAML configs loaded at runtime
├── sql/schemas/          ← table CREATE scripts
├── sql/views/            ← ActionableSignal.sql and other views
├── scripts/              ← analysis, comparison, and batch scripts
└── tests/
```

Key rule: `assets/` must stay flat — Dagster breaks if assets live in subdirectories.

Key rule: all Python code must be written to `.py` files before execution — never `python -c "..."` inline (breaks approval flow in Cursor/Antigravity).

---

## Architecture decisions (permanent record)

| Decision | Rule |
|---|---|
| ActionableSignal is a VIEW | Never delete from the VIEW. Delete from `PotentialPrediction` directly. The VIEW reflects automatically. |
| STT transcript bucket hardcoded | `gor-stt-transcripts` is hardcoded in `definitions.py` line 206. Different from `GCS_BUCKET_NAME` (which is `gor-media-prod`). |
| Load jobs only, no streaming inserts | Streaming inserts trigger a buffer that prevents row deletion. Always use BigQuery load jobs. |
| `in_process_executor` for DNS-sensitive jobs | Multiprocess child workers fail to resolve Dagster Cloud hostname. Use `in_process_executor` for affected jobs. |
| All Dagster packages pinned, upgraded together | `dagster`, `dagster-cloud`, `dagster-gcp`, `dagster-postgres` all at 1.10.19. Never upgrade one without the others. |
| `max_concurrent: 2` default | Prevents GCE VM resource contention. Override explicitly per job when needed. |
| BigQuery as system of record, Supabase as read-replica | All writes go to BigQuery. Supabase is synced nightly. App reads Supabase, never writes to synced tables. |
| UTC timestamps everywhere | All timestamps must use UTC. `+00:00` timezone indicator is mandatory. |
| Signal date uses `ContentSourceTimingConfig` | `pubDate` from RSS is not air date. Each content source has a timing strategy: `scheduled_title_date`, `publication_driven`, etc. |
| Performance truncation includes implicit flip | Same finfluencer + instrument: opposite-direction `start_*`/`hold_*` truncates prior position; see [[concepts/signal-performance]]. |
| No mocks in tests | All tests must use real APIs, real BigQuery, real GCS. No mocks, stubs, or test doubles for external services. |
| Dagster Cloud credit hygiene | Tune sensors/schedules and `RunRequest` fan-out for hybrid orchestration credits; permanent write-up: [dagster-cloud-credit-optimization.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/dagster-cloud-credit-optimization.md). |
| BigQuery bytes + PotentialPrediction guardrail | Partition predicates on hot reads; **`require_partition_filter`** on production `PotentialPrediction`; local lint `check_no_select_star.py`; checklist [bigquery-cost-checklist.md](file:///Users/andreskull/gor_dagster/docs/operations/bigquery-cost-checklist.md); program summary [bigquery-cost-optimization.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/bigquery-cost-optimization.md). |
| Resolution re-attempt + JW matcher | Frozen PIR/PSR re-enter only when `reattempt_eligible=TRUE`. JW instrument matcher; speaker fuzzy **0.935**; org-conflict override off. Alias on every auto-resolve. [[concepts/resolution-pipeline-efficiency]] |
| Curation learning (P1/P2/P3) | Fund-noise similarity always on; unique-ticker bar **0.85**; Stage **0.75** promotion (≥2 manual curations). Sensor flags on **2026-07-07**. [[concepts/curation-learning]] |
| Transcript hydration canonical utils + FR-6 | Hot path in `transcript_hydration_utils.py`; O(U log W + W) index; `MEMBERSHIP_TOL=0.01`; asset re-exports only. Golden speaker accuracy unchanged (scorer bypasses hydration artifact). [hydration-performance-optimization.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/hydration-performance-optimization.md) |
| SPY single benchmark FIGI | SPY/US canonical FIGI **`BBG000BDTBL9`**; FI UUID unchanged; dashboard FIGI-first + ticker/exchange fallback; daily `spy_single_identity` asset check; performance exit uses as-of price join. [spy-canonical-figi-consolidation.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/spy-canonical-figi-consolidation.md) |
| Podcast RSS SI discovery | `si_sensor` queries all active `podcast_rss` ContentSources via `get_podcast_rss_content_source_ids()` — no per-onboarding allowlist (2026-07-01). [hidden-gems-ingestion.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/hidden-gems-ingestion.md) |
| Pattern 6 Megaphone ARML | `extract_episode_id_from_enclosure_url()` branch for `feeds.megaphone.fm/{slug}` shows with `ARML{digits}.mp3`; Megaphone slug valid as `ContentSource.external_id`. [hidden-gems-ingestion.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/hidden-gems-ingestion.md). Reused without code change for Investing Unscripted (**2026-07-27**). |
| Finfluencers Directory on every podcast onboarding | Playbook Requirement 13 / PR 5: revise gor-blog `docs/directory/index.md` (+ `_profiles.json` for Covered). [onboarding-new-podcast-source.md](file:///Users/andreskull/gor_dagster/docs/operations/onboarding-new-podcast-source.md); example [investing-unscripted-ingestion.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/investing-unscripted-ingestion.md) |
| ElevenLabs mono-speaker SI hardening | Resplit at unify (120s windows); auto-heal at SI load from GCS raw (`elevenlabs_unified_heal.py`); healable stuck exclusion; merged-segment coverage via `merge_metadata.merged_utterances`. [hidden-gems-ingestion.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/hidden-gems-ingestion.md) |
| Recursive LLM extraction (memory-centric SI/FE) | Algorithm wrapped **2026-07-01**; production model **`si-gem35fl-recursive`** (@450s) / **`fe-gem35fl-recursive`** (@1800s) since **2026-07-27**. Batch backlog delivery: `docs/features/batch-integration/` Phase 5. [recursive-llm-extraction.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/recursive-llm-extraction.md) |
| Gemini 3.5 Flash-Lite migration | Dual cost/quality gate; SI cost waiver @450; four ladder surfaces distinct; ActionableSignal `fe-gem35fl-recursive` priority 1; dashboard quality-by-family. Wrapped **2026-07-27**. [gemini-35-flash-lite-migration.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/gemini-35-flash-lite-migration.md) |
| ContentItem load-job idempotency | RSS batch dedupe + `content_item_insert_already_present` guard prevents duplicate physical rows on load-job retry (2026-06-30). Extends [contentitem-dedupe-and-cleanup.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/contentitem-dedupe-and-cleanup.md) |
| CNBC IPO scoreboard (SPCX v1) | BQ snapshot mats → Supabase RPC `get_ipo_scoreboard_page`; since-call perf SQL; public `/cnbc-ipo`; **kept picks only** in UI; social + blog deferred. [cnbc-ipo-scoreboard.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/cnbc-ipo-scoreboard.md) |
| LinkedIn enrichment (trust-tiered) | Discovery never auto-writes profiles; trusted-only Supabase sync; published audit 100% trusted-or-none; no third-party LinkedIn API (2026-07-23). [[concepts/linkedin-enrichment]] |

---

## Completed architecture features

Permanent docs under `docs/architecture/features/` (post-`/wrapup`).

| Completed | Topic | Doc |
|---|---|---|
| 2026-05-17 | BigQuery cost optimization (partition guardrails, `SELECT *` lint, cost snapshots under `docs/operations/`) | [bigquery-cost-optimization.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/bigquery-cost-optimization.md) |
| 2026-05-30 | 7investing RSS onboarding (Anchor/Spotify; Patterns 4+5; SI monitored set) | [7investing-ingestion.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/7investing-ingestion.md) |
| 2026-06-10 | Resolution pipeline efficiency (JW matcher, re-attempt, sweeps, alias-on-resolve) | [resolution-pipeline-efficiency.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/resolution-pipeline-efficiency.md) |
| 2026-06-15 | Transcript hydration performance optimization (O(U·W)→O(U log W + W), FR-6 membership, canonical utils) | [hydration-performance-optimization.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/hydration-performance-optimization.md) |
| 2026-06-15 | The Acquirers Podcast RSS onboarding (Anchor/Spotify; Patterns 4+5; 8th SI source; 436 episodes) | [acquirers-podcast-ingestion.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/acquirers-podcast-ingestion.md) |
| 2026-06-30 | SPY canonical FIGI consolidation (BL9 identity, dashboard fallback, guardrail, as-of performance exit) | [spy-canonical-figi-consolidation.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/spy-canonical-figi-consolidation.md) |
| 2026-07-07 | Curation learning — fund-noise similarity, unique-ticker bar, Stage 0.75 promotion | [curation-learning.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/curation-learning.md) |
| 2026-07-01 | Proof-segment speaker resolution (gate live, curation + backfill + sync verified) | [proof-segment-speaker-resolution.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/proof-segment-speaker-resolution.md) |
| 2026-07-01 | Recursive LLM extraction — memory-centric SI/FE (wrapped) | [recursive-llm-extraction.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/recursive-llm-extraction.md) |
| 2026-07-01 | Motley Fool Hidden Gems RSS onboarding (Pattern 6 ARML, Megaphone slug, STT pacing, source-agnostic SI; 2247 episodes) | [hidden-gems-ingestion.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/hidden-gems-ingestion.md) |
| 2026-07-11 | CNBC IPO scoreboard — SPCX public page, BQ→Supabase sync, OG/SEO; social + blog deferred | [cnbc-ipo-scoreboard.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/cnbc-ipo-scoreboard.md) |
| 2026-07-23 | LinkedIn enrichment — discovery + curation + published audit; trusted-only frontend sync; no third-party API | [linkedin-enrichment.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/linkedin-enrichment.md) |
| 2026-07-27 | Gemini 3.5 Flash-Lite migration — SI @450 / FE @1800; LinkedIn + tk slice; dashboard quality-by-family | [gemini-35-flash-lite-migration.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/gemini-35-flash-lite-migration.md) |
| 2026-07-27 | Investing Unscripted RSS onboarding (Pattern 6 twin; directory Covered; 272/272 downloaded) | [investing-unscripted-ingestion.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/investing-unscripted-ingestion.md) |
| 2026-05-15 | Pytest `not expensive` green track (permanent reference; suite alignment) | [pytest-not-expensive-green.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/pytest-not-expensive-green.md) |
| 2026-05-14 | ContentItem deduplication, ingest guard, BQ apply pipeline | [contentitem-dedupe-and-cleanup.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/contentitem-dedupe-and-cleanup.md) — runbook [contentitem-dedupe-runbook.md](file:///Users/andreskull/gor_dagster/docs/operations/contentitem-dedupe-runbook.md) |
| 2026-05-14 | Compound and Friends (Pippa) RSS onboarding + SI allowlist extension | [compound-and-friends-ingestion.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/compound-and-friends-ingestion.md) |
| 2026-05-12 | Morning Filter RSS onboarding (third podcast source) | [morning-filter-ingestion.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/morning-filter-ingestion.md) |
| 2026-04-25 | Dagster Cloud credit optimization | [dagster-cloud-credit-optimization.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/dagster-cloud-credit-optimization.md) |
| 2026-05-04 | Signal pruning & hybrid performance serving | [signal-pruning-performance.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/signal-pruning-performance.md) |
| 2026-05-14 | DeepSeek-V4-Flash SI/FE production migration | [deepseek-v4-flash-migration.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/deepseek-v4-flash-migration.md) |

---

## Active features in progress

These live in **`gor_dagster/docs/features/`** — temporary until `/wrapup`; not indexed verbatim by wiki.

| Feature | Folder / notes |
|---|---|
| Batch LLM integration | `batch-integration/` — Phase 1 done (`LLMBatchProcessor`); Phase 5 = recursive SI wave batch (migrated from recursive-llm-extraction wrapup) |
| Finfluencer and show profiles | `finfluencer-and-show-profiles/` |
| Social share previews | `social-share-previews/` |
| Post-cutoff IPO resolution | `post-cutoff-ipo-resolution/` — Inc 1–8 backfill gate ✅ (2026-06-28); frozen DATA_REFRESH curation ongoing |
| LinkedIn outreach intros | `linkedin-outreach-intro-personalization/` — tasks complete; wrapup pending |

**Wrapped 2026-07-27:** `investing-unscripted-ingestion/` → [investing-unscripted-ingestion.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/investing-unscripted-ingestion.md) (272/272 downloaded; Pattern 6 twin; gor-blog Covered `investing-unscripted`; STT draining; app `/show/` awaits `mat_shows`)

**Wrapped 2026-07-27:** `gemini-35-flash-lite-migration/` → [gemini-35-flash-lite-migration.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/gemini-35-flash-lite-migration.md) (SI @450 / FE @1800; ActionableSignal priority 1; dashboard Model Quality Comparison)

**Wrapped 2026-07-23:** `linkedin-enrichment/` → [linkedin-enrichment.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/linkedin-enrichment.md) (trust-tiered discovery; published 100% trusted-or-none; no third-party API)

**Wrapped 2026-07-11:** `cnbc-ipo-scoreboard-social/` → [cnbc-ipo-scoreboard.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/cnbc-ipo-scoreboard.md) (live **`https://finfluencers.trade/cnbc-ipo`**; SPCX v1; increments 7–8 social/blog deferred; monitor pick frequency before expansion)

**Wrapped 2026-07-01:** `recursive-llm-extraction/` → [recursive-llm-extraction.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/recursive-llm-extraction.md) (interactive **`si-gem31fl-recursive`** / **`fe-gem31fl-recursive`** in prod; 60-episode Compound list-2 rescue complete). Unimplemented Vertex AI batch delivery moved to **`batch-integration/`** Phase 5.

**Wrapped 2026-07-01:** `proof-segment-speaker-resolution/` → [proof-segment-speaker-resolution.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/proof-segment-speaker-resolution.md) (gate live; 73,261 resolved; mat_signals + Supabase verified; ops scripts retained)

**Wrapped 2026-07-01:** `motley-fool-hidden-gems-ingestion/` → [hidden-gems-ingestion.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/hidden-gems-ingestion.md) (2247/2247 downloaded; STT backfill ongoing; Pattern 6; `si_sensor` now source-agnostic; ElevenLabs mono-speaker resplit + auto-heal + merged-segment coverage fix documented in permanent doc)

**Wrapped 2026-06-30:** `spy-canonical-figi-consolidation/` → [spy-canonical-figi-consolidation.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/spy-canonical-figi-consolidation.md)

**Wrapped 2026-06-15:** `hydration-performance-optimization/` → [hydration-performance-optimization.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/hydration-performance-optimization.md)

**Wrapped 2026-06-15:** `acquirers-podcast-ingestion/` → [acquirers-podcast-ingestion.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/acquirers-podcast-ingestion.md)

**Wrapped 2026-06-10:** `resolution-pipeline-efficiency/` → [resolution-pipeline-efficiency.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/resolution-pipeline-efficiency.md)

---

## Operations reference

| Topic | Guide |
|---|---|
| BigQuery cost snapshots + checklist | [cost-snapshots README](file:///Users/andreskull/gor_dagster/docs/operations/cost-snapshots/README.md), [bigquery-cost-checklist.md](file:///Users/andreskull/gor_dagster/docs/operations/bigquery-cost-checklist.md) |
| RSS ingestion | [RSS Ingestion Guide](file:///Users/andreskull/gor_dagster/docs/operations/rss-ingestion-guide.md) |
| ContentItem duplicate remediation | [ContentItem dedupe runbook](file:///Users/andreskull/gor_dagster/docs/operations/contentitem-dedupe-runbook.md) |
| New podcast source onboarding | [Onboarding a New Podcast Source — Playbook](file:///Users/andreskull/gor_dagster/docs/operations/onboarding-new-podcast-source.md) — [[concepts/onboarding-new-podcast-source]] |
| STT batch processing | [STT Batch Processing Guide](file:///Users/andreskull/gor_dagster/docs/operations/stt-batch-processing-guide.md) |
| STT provider selection | [STT Providers Guide](file:///Users/andreskull/gor_dagster/docs/operations/stt-providers-guide.md) |
| STT monitoring | [STT Monitoring](file:///Users/andreskull/gor_dagster/docs/operations/stt-monitoring.md) |
| Speaker attribution | [Speaker Attribution Guide](file:///Users/andreskull/gor_dagster/docs/operations/speaker-attribution-guide.md) |
| Speaker resolution | [Speaker Resolution Workflow](file:///Users/andreskull/gor_dagster/docs/operations/speaker-resolution-workflow.md) |
| Proof-segment speaker resolution | [proof-segment-speaker-resolution.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/proof-segment-speaker-resolution.md); verify: `proof_segment_integration_checks.py`, `verify_mat_signals_proof_segments.py`, `verify_supabase_proof_segments.py` |
| Golden reference creation | [Golden Reference Workflow](file:///Users/andreskull/gor_dagster/docs/operations/golden-reference-workflow.md) |
| Facts extraction | [Facts Extraction Guide](file:///Users/andreskull/gor_dagster/docs/operations/facts-extraction-guide.md) |
| Instrument resolution | [FIGI Instrument Cleanup Guide](file:///Users/andreskull/gor_dagster/docs/operations/figi-instrument-cleanup-guide.md) |
| Resolution backlog monitoring | `scripts/analyze_resolution_backlog.py`, `scripts/analyze_resolution_drill.py` — see [resolution-pipeline-efficiency.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/resolution-pipeline-efficiency.md) |
| CNBC IPO scoreboard (SPCX) | [cnbc-ipo-scoreboard.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/cnbc-ipo-scoreboard.md); verify: `scripts/verify_ipo_scoreboard_rpc_prod.py`, `scripts/analyze_hot_ipo_scoreboard_candidates.py` |
| LinkedIn enrichment / audit CSV | [linkedin-enrichment.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/linkedin-enrichment.md); [linkedin-audit-csv-agent-instructions.md](file:///Users/andreskull/gor_dagster/docs/operations/linkedin-audit-csv-agent-instructions.md); `scripts/linkedin_coverage_cost_report.py`, `scripts/verify_linkedin_supabase_sync.py` |
| Gemini 3.5 Flash-Lite promote / rollback | [gemini-35-flash-lite-migration.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/gemini-35-flash-lite-migration.md); decision [promotion_decision.md](file:///Users/andreskull/gor_dagster/docs/analytics/gemini-35-flash-lite-migration/promotion_decision.md); [rollback_checklist.md](file:///Users/andreskull/gor_dagster/docs/analytics/gemini-35-flash-lite-migration/rollback_checklist.md) |
| SPY benchmark identity verification | [spy-figi-consolidation-runbook.md](file:///Users/andreskull/gor_dagster/docs/operations/spy-figi-consolidation-runbook.md), `scripts/verify_spy_single_identity.py` |
| Schema management | [Schema Management](file:///Users/andreskull/gor_dagster/docs/operations/schema-management.md) |
| Configuration reference | [Configuration Reference](file:///Users/andreskull/gor_dagster/docs/operations/configuration-reference.md) |
| Price ingestion | [Price Ingestion Guide](file:///Users/andreskull/gor_dagster/docs/operations/price-ingestion-guide.md) |
| Dagster schedules (prod vs code defaults) | [Dagster schedules — production](file:///Users/andreskull/gor_dagster/docs/operations/dagster-schedules-production.md) |
| SI job reliability / credits | [SI job reliability](file:///Users/andreskull/gor_dagster/docs/operations/si-job-reliability.md) |

---

## WIKI.md

[WIKI.md](file:///Users/andreskull/gor_dagster/WIKI.md) — project entry point for wiki syncs. Update it when architecture changes.

## Related pages

- [[products/finfluencer-trade]]
- [[projects/gor-blog]]
- [[concepts/proof-segment-speaker-resolution]]
- [[concepts/speaker-attribution]]
- [[concepts/actionable-signal]]
- [[concepts/signal-performance]]
- [[concepts/llm-config-registry]]
- [[concepts/onboarding-new-podcast-source]]
- [[concepts/resolution-pipeline-efficiency]]
- [[concepts/linkedin-enrichment]]
- [[entities/dagster]]
- [[entities/bigquery]]
