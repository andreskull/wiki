---
type: concept
title: "Onboarding a new podcast source"
product: finfluencer-trade
project: gor_dagster
created: 2026-05-13
updated: 2026-07-01
tags: [rss, dagster, content-source, pipeline, si-sensor, gor_dagster]
---

# Onboarding a new podcast source

## Definition

Standard **playbook** for adding a new podcast RSS feed to [[projects/gor_dagster]] so episodes flow end-to-end: RSS → `ContentItem` → audio in GCS → STT → speaker identification (SI) → hydration → facts extraction (FE) → `ActionableSignal`. It formalises rollout as preflight → optional regex PR → operational `ContentSource` creation → post-deploy monitoring and wrap-up.

## Canonical document

Full text (pipeline diagram, gate details, requirements/tasks templates, case studies, living lessons log, file pointers): [onboarding-new-podcast-source.md](file:///Users/andreskull/gor_dagster/docs/operations/onboarding-new-podcast-source.md)

## Relevance

New shows are recurring work. Without a checklist, two failure modes repeat: **episode ID extraction** fails for unfamiliar enclosure URL shapes, and episodes stall after STT if SI never runs. Since **2026-07-01**, standard **`podcast_rss`** sources are picked up automatically by `get_podcast_rss_content_source_ids()` in `si_sensor` — Gate 2 (manual SI allowlist) is **no longer required** for normal podcast onboarding. Gate 1 (regex) remains mandatory when preflight exit code is **2**.

## Three gates (summary)

| Gate | Symptom if missed | Typical fix |
|------|-------------------|-------------|
| **1** — `extract_episode_id_from_enclosure_url` | `ValueError` on every episode | New regex branch **last** in `rss_ingestion.py` + tests (e.g. **Pattern 6** Megaphone `ARML…` for direct `feeds.megaphone.fm/{slug}` feeds) |
| **2** — SI sensor source discovery | No `stt_speaker_attributions` despite green RSS/STT | **Resolved for `podcast_rss`** — `si_sensor` queries all active podcast RSS ContentSources. Verify `source_type = 'podcast_rss'` on the new row. |
| **3** — backfill ordering | Rare: old episodes ingest first when limits apply | Usually N/A when `MAX_BACKFILL_EPISODES=-1`; override if feed is oldest-first |

**Preflight discipline:** classify enclosure URLs **after following redirects** — RSS host and audio CDN vendor often differ (e.g. Pippa RSS → Megaphone CDN; Megaphone direct RSS → podtrac → `traffic.megaphone.fm`).

## Rollout shape

Four PRs plus wrap-up: **Preflight script** → **Regex + validator** (conditional) → **Operational** `add_new_rss_source_job` → **docs + verification scripts** → `/wrapup` / wiki sync. Large catalogs may need **STT credit pacing** across multiple ElevenLabs billing cycles (see [hidden-gems-ingestion.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/hidden-gems-ingestion.md)).

## Which projects use this

- [[projects/gor_dagster]] — implementation home
- [[products/finfluencer-trade]] — product outcome (`ContentSource` / signals)

## Related pages

- [[projects/gor_dagster]] — Stage 1 RSS ingestion and operations table
- [[entities/dagster]] — orchestration context
- [[concepts/actionable-signal]] — downstream VIEW gates
