---
type: concept
title: "Onboarding a new podcast source"
product: finfluencer-trade
project: gor_dagster
created: 2026-05-13
updated: 2026-08-22
tags: [rss, dagster, content-source, pipeline, si-sensor, gor_dagster, gor-blog]
---

# Onboarding a new podcast source

## Definition

Standard **playbook** for adding a new podcast RSS feed to [[projects/gor_dagster]] so episodes flow end-to-end: RSS → `ContentItem` → audio in GCS → STT → speaker identification (SI) → hydration → facts extraction (FE) → `ActionableSignal`. It formalises rollout as preflight → optional regex PR → operational `ContentSource` creation → **gor-blog Finfluencers Directory revision** → post-deploy monitoring and wrap-up.

## Canonical document

Full text (pipeline diagram, gate details, requirements/tasks templates, case studies, living lessons log, file pointers): [onboarding-new-podcast-source.md](file:///Users/andreskull/gor_dagster/docs/operations/onboarding-new-podcast-source.md)

## Relevance

New shows are recurring work. Without a checklist, two failure modes repeat: **episode ID extraction** fails for unfamiliar enclosure URL shapes, and episodes stall after STT if SI never runs. Since **2026-07-01**, standard **`podcast_rss`** sources are picked up automatically by `get_podcast_rss_content_source_ids()` in `si_sensor` — Gate 2 (manual SI allowlist) is **no longer required** for normal podcast onboarding. Gate 1 (regex) remains mandatory when preflight exit code is **2**.

Since **2026-07-27**, every onboarding **must** revise the public Finfluencers Directory on [[projects/gor-blog]] (playbook Requirement 12 / PR 5): add or update `docs/directory/index.md`, and add `_profiles.json` when the show should appear under **Covered on finfluencers.trade**. Example: [investing-unscripted-ingestion.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/investing-unscripted-ingestion.md).

## Three gates (summary)

| Gate | Symptom if missed | Typical fix |
|------|-------------------|-------------|
| **1** — `extract_episode_id_from_enclosure_url` | `ValueError` on every episode | New regex branch **last** in `rss_ingestion.py` + tests (e.g. **Pattern 6** Megaphone `ARML…` or **Pattern 7** `PPLLC…` for direct `feeds.megaphone.fm/{slug}` feeds) |
| **2** — SI sensor source discovery | No `stt_speaker_attributions` despite green RSS/STT | **Resolved for `podcast_rss`** — `si_sensor` queries all active podcast RSS ContentSources. Verify `source_type = 'podcast_rss'` on the new row. |
| **3** — backfill ordering | Rare: old episodes ingest first when limits apply | Usually N/A when `MAX_BACKFILL_EPISODES=-1`; override if feed is oldest-first |

**Preflight discipline:** classify enclosure URLs **after following redirects** — RSS host and audio CDN vendor often differ (e.g. Pippa RSS → Megaphone CDN; Megaphone direct RSS → podtrac → `traffic.megaphone.fm`).

## Rollout shape

Pipeline PRs plus directory + wrap-up: **Preflight script** → **Regex + validator** (conditional) → **Operational** `add_new_rss_source_job` → **docs + SI discovery verification** → **gor-blog directory** (required) → monitoring → `/wrapup` / wiki sync. Large catalogs may need **STT credit pacing** across multiple ElevenLabs billing cycles (see [hidden-gems-ingestion.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/hidden-gems-ingestion.md)).

**Ops tips (2026-07-27):** Launch `add_new_rss_source_job` from **Jobs** with flat job-level YAML (no `ops:` wrapper). Local BigQuery against `dagster_prod` uses location **`europe-north1`**. Covered `_profiles.json` can ship before Supabase `shows` sync (app `/show/{slug}` 404s until `mat_shows` has ActionableSignals).

**Ops tips (2026-08-01 / Chit Chat Stocks):** Do **not** require a manual `add_new_rss_source_job` re-launch for idempotency — retired from the playbook checklist. When only an Apple Podcasts ID is known, preflight may discover `feedUrl` via iTunes lookup (`itunes.apple.com/lookup?id=…`) and still derive `external_id` from the host feed slug (never the Apple ID). Example: [chit-chat-stocks-ingestion.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/chit-chat-stocks-ingestion.md) (11th `podcast_rss` source; Pattern 6; Covered mapping deferred at wrap-up).

**Ops tips (2026-08-19):** Do **not** add a playbook Requirement for idempotency / safe re-runs (former Requirement 9). Proven across prior shows. Directory revision is Requirement 12.

**Ops tips (2026-08-22 / The Intrinsic Value Podcast):** Megaphone `PPLLC{digits}.mp3` is **Pattern 7** (not ARML Pattern 6). Trim iTunes network suffixes on `ContentSource.name` **before** the first `mat_shows` sync — slugs freeze after insert. Example: [intrinsic-value-podcast-ingestion.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/intrinsic-value-podcast-ingestion.md) (12th `podcast_rss` source; 784/784 downloaded; Covered `the-intrinsic-value-podcast`).

## Which projects use this

- [[projects/gor_dagster]] — implementation home
- [[projects/gor-blog]] — Finfluencers Directory revision (required every onboarding)
- [[products/finfluencer-trade]] — product outcome (`ContentSource` / signals / directory)

## Related pages

- [[projects/gor_dagster]] — Stage 1 RSS ingestion and operations table
- [[projects/gor-blog]] — directory content
- [[entities/dagster]] — orchestration context
- [[concepts/actionable-signal]] — downstream VIEW gates
