---
type: concept
title: "Onboarding a new podcast source"
product: finfluencer-trade
project: gor_dagster
created: 2026-05-13
updated: 2026-05-13
tags: [rss, dagster, content-source, pipeline, si-sensor, gor_dagster]
---

# Onboarding a new podcast source

## Definition

Standard **playbook** for adding a new podcast RSS feed to [[projects/gor_dagster]] so episodes flow end-to-end: RSS → `ContentItem` → audio in GCS → STT → speaker identification (SI) → hydration → facts extraction (FE) → `ActionableSignal`. It formalises rollout as preflight → optional regex PR → operational `ContentSource` creation → SI sensor list update → post-deploy monitoring and wrap-up.

## Canonical document

Full text (pipeline diagram, gate details, requirements/tasks templates, case studies, living lessons log, file pointers): [onboarding-new-podcast-source.md](file:///Users/andreskull/gor_dagster/docs/operations/onboarding-new-podcast-source.md)

## Relevance

New shows are recurring work. Without a checklist, two failure modes repeat: **episode ID extraction** fails for unfamiliar enclosure URL shapes, and the **SI sensor’s hardcoded `PIPELINE_SI_MONITORED_CONTENT_SOURCE_IDS`** omits the new `content_source_id` so STT succeeds but SI never runs — a silent gap. The playbook names these as explicit gates and prescribes tests and PR boundaries.

## Three gates (summary)

| Gate | Symptom if missed | Typical fix |
|------|-------------------|-------------|
| **1** — `extract_episode_id_from_enclosure_url` | `ValueError` on every episode | New regex branch **last** in `rss_ingestion.py` + tests |
| **2** — SI sensor source list | No `stt_speaker_attributions` despite green RSS/STT | Add `content_source_id` to monitored list + regression test |
| **3** — backfill ordering | Rare: old episodes ingest first when limits apply | Usually N/A when `MAX_BACKFILL_EPISODES=-1`; override if feed is oldest-first |

**Preflight discipline:** classify enclosure URLs **after following redirects** — RSS host and audio CDN vendor often differ (e.g. Pippa RSS → Megaphone CDN).

## Rollout shape

Four PRs plus wrap-up: **Preflight script** → **Regex** (conditional) → **Operational** `add_new_rss_source_job` → **SI sensor + docs** → monitoring and `/wrapup` / wiki sync.

## Which projects use this

- [[projects/gor_dagster]] — implementation home
- [[products/finfluencer-trade]] — product outcome (`ContentSource` / signals)

## Related pages

- [[projects/gor_dagster]] — Stage 1 RSS ingestion and operations table
- [[entities/dagster]] — orchestration context
- [[concepts/actionable-signal]] — downstream VIEW gates
