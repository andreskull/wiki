---
type: decision
title: "New transcriptions use ElevenLabs Scribe v3"
product: finfluencer-trade
project: gor_dagster
created: 2026-11-14
updated: 2026-11-14
tags: [stt, elevenlabs, scribe, pipeline]
---

# New transcriptions use ElevenLabs Scribe v3

## Context

Scribe v3 was compared with v2 on four golden episodes, three speaker-identification runs each. Mean overall was 0.7449 for v3 and 0.7259 for v2. The strict bar was a gap of 0.02, and the run-to-run spread was 0.0304. Keyword capture on episode `6d3b393e` stayed lower for v3.

## Options considered

Stay on v2. Or switch new transcriptions to v3 and keep v1 and v2 transcripts valid.

## Decision made

Go, on 2026-11-14. New transcriptions use `elevenlabs_scribe_v3`. The 0.019 mean gap was accepted. The keyword miss was left as a real miss.

## Consequences

Source order is v3, then v2, then v1. The speaker-identification sensor discovers v3 only. Episodes that already have a v1 or v2 transcript are not transcribed again unless `transcribe_v3_when_older_exists` is true. Production has been writing `scribe_v3` rows since the deploy of merge `0ae45468`.

A rollback sets `DEFAULT_STT_PROVIDER_STAGING` and `DEFAULT_STT_PROVIDER_PROD` back to v2, and needs a code change and a deploy for the priority and the sensor list. v3 stays in the source-priority list. No data migration.

## Affected projects

`gor_dagster` only. No Supabase schema change.

## Related pages

- [[projects/gor_dagster]]
- [[concepts/speaker-attribution]]
- [[entities/dagster]]
- [[entities/gcs]]
