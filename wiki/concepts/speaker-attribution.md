---
type: concept
title: "Speaker Attribution"
product: finfluencer-trade
project: gor_dagster
created: 2026-04-06
updated: 2026-07-01
tags: [speaker-attribution, stt, llm, transcript, diarization, elevenlabs]
---

# Speaker Attribution

The process of identifying and naming speakers in a podcast transcript. A two-stage pipeline in [[projects/gor_dagster]] that transforms raw diarized transcripts (Speaker 0, Speaker 1…) into transcripts labelled with real names.

## How it works

**Stage 1 — LLM Identification:** Production uses **memory-centric recursive SI** (`si-gem31fl-recursive`, shipped **2026-06-15**, wrapped **2026-07-01**): three-wave algorithm — roster bootstrap (1 call) → parallel 300s window passes (delta-only ops against live speaker memory) → optional resolution → deterministic fold. Show priors from BigQuery seed bootstrap; evidence-gated merges rewrite labels retroactively. Output: identification file at `gs://gor-stt-transcripts/identification/{episode_id}/{provider}_{llm_config_id}.json`. Legacy configs (`si-dsv4fr-58k`, grok-era) remain as artifact-priority compat tails.

Single-pass mode and eight+ LLM config families remain available via the registry (Gemini, GPT, Claude, etc.).

**Pending:** Vertex AI batch delivery for recursive SI backlog cohorts — `gor_dagster/docs/features/batch-integration/` Phase 5 (~50% interactive cost). Permanent doc: [recursive-llm-extraction.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/recursive-llm-extraction.md).

**Stage 2 — Hydration:** A non-LLM process combines the identification file with the original transcript to produce a fully-attributed "hydrated" transcript. Written to `gs://gor-stt-transcripts/hydrated/{episode_id}/{provider}_{llm_config_id}.json`.

**Canonical implementation (2026-06-15):** `gor_dagster/utils/transcript_hydration_utils.py` — the asset imports/re-exports only. Hot path uses `WordIndex` + normalization cache: **O(U log W + W)** per episode (was O(U·W) nested scans). Production index builder: **FR-6** start-anchored word membership with `MEMBERSHIP_TOL=0.01` (recovers boundary/short utterances and speaker-disagreement words). Golden speaker accuracy scorer does **not** read hydration output — it builds config utterances from SI segments + unified text, so FR-6 cannot regress golden speaker accuracy by construction.

**Performance:** Large fixture (1,295 utterances, 18,129 words): baseline ~2.93s → ~0.09s median in-memory (~33×). **Orchestration:** `hydration_sensor` batches up to 30 `(episode, llm_config_id)` pairs per tick; audit in `hydration_runs` BigQuery table.

**Stage 3 — Statistical Evaluation:** Statistical consensus analysis flags deviations for human review. Human corrections feed into progressive "golden reference" transcripts (v1 → v2 → v3) used to evaluate and improve future attribution runs.

## ElevenLabs mono-speaker hardening (2026-07-01)

ElevenLabs Scribe mono-diarization can collapse an entire episode into one or few mega-utterances (~18 min). Recursive SI then fails with empty segments; even after resplit, `SegmentMerger` can merge same-speaker utterances and the **70% duration coverage gate** under-counts unless merged spans are expanded.

Pipeline mitigations (show-agnostic):

1. **Resplit at unify** — `unified_transcript_normalizers.py` splits into 120s windows when triggered (utterance >480s or mono share ≥85%).
2. **Auto-heal at SI load** — `elevenlabs_unified_heal.py` re-unifies from GCS raw (no STT re-call; max 2 attempts); wired in `speaker_attribution_llm.py`.
3. **Healable stuck exclusion** — `si_sensor` does not count healable mega-utterance failures toward `si_stuck`.
4. **Merged-segment coverage** — `llm_coverage_calculator.calculate_unique_time_covered()` uses `merge_metadata.merged_utterances` when present.

Permanent reference: [hidden-gems-ingestion.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/hidden-gems-ingestion.md) §ElevenLabs mono-speaker SI hardening.

## Key constraints

- **Anchor enforcement:** Real-name attributions require explicit textual evidence (hard anchors) in the transcript. Without an anchor, the LLM must use `Unknown`.
- **Proof-segment speaker gate:** `ActionableSignal` rows require `proof_segments_speaker_status = 'resolved'`. Secondary quote speakers resolve via unified dashboard queue (`resolve_proof_segment_speaker`); sentinel finfluencers unblock diarization artifacts and single-token anonymous names. Wrapped **2026-07-01** — [[concepts/proof-segment-speaker-resolution]].

## Projects using it

- [[projects/gor_dagster]] — core implementation

## Related pages

- [Recursive LLM extraction](file:///Users/andreskull/gor_dagster/docs/architecture/features/recursive-llm-extraction.md)
- [[concepts/proof-segment-speaker-resolution]]
- [Proof-segment speaker resolution](file:///Users/andreskull/gor_dagster/docs/architecture/features/proof-segment-speaker-resolution.md)
- [[concepts/actionable-signal]]
- [[concepts/llm-config-registry]]
- [[entities/assemblyai]]
- [[entities/dagster]]
- [Transcript Hydration Architecture](file:///Users/andreskull/gor_dagster/docs/architecture/transcript-hydration-architecture.md)
- [Hydration performance optimization](file:///Users/andreskull/gor_dagster/docs/architecture/features/hydration-performance-optimization.md)
- [Hidden Gems ingestion — ElevenLabs SI hardening](file:///Users/andreskull/gor_dagster/docs/architecture/features/hidden-gems-ingestion.md)
