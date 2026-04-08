---
type: concept
title: "Speaker Attribution"
product: finfluencer-trade
project: gor_dagster
created: 2026-04-06
updated: 2026-04-06
tags: [speaker-attribution, stt, llm, transcript, diarization]
---

# Speaker Attribution

The process of identifying and naming speakers in a podcast transcript. A two-stage pipeline in [[projects/gor_dagster]] that transforms raw diarized transcripts (Speaker 0, Speaker 1…) into transcripts labelled with real names.

## How it works

**Stage 1 — LLM Identification:** An LLM reads the full unified transcript and produces a lightweight "identification file" containing speaker mappings and utterance split information — no transcript text. Supports 8 LLM configs (Gemini 2.5-flash/2.0-flash/2.5-pro, GPT-5, GPT-4o, GPT-4o-mini, O3-mini, Claude Sonnet 4).

**Stage 2 — Hydration:** A non-LLM process combines the identification file with the original transcript to produce a fully-attributed "hydrated" transcript.

**Stage 3 — Statistical Evaluation:** Statistical consensus analysis flags deviations for human review. Human corrections feed into progressive "golden reference" transcripts (v1 → v2 → v3) used to evaluate and improve future attribution runs.

## Key constraints

- **Anchor enforcement:** Real-name attributions require explicit textual evidence (hard anchors) in the transcript. Without an anchor, the LLM must use `Unknown`.
- **Proof-segment speaker gate:** `ActionableSignal` rows are only visible when `proof_segments_speaker_status = 'resolved'`.
- **Unknown handling:** Unknown speakers are tracked and resolved separately through a human review workflow (`PendingSpeakerResolution`).

## Projects using it

- [[projects/gor_dagster]] — core implementation

## Related pages

- [[concepts/actionable-signal]]
- [[concepts/llm-config-registry]]
- [[entities/assemblyai]]
- [[entities/dagster]]
