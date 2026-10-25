---
type: concept
title: "Episode-Title Speaker Hint"
created: 2026-10-25
updated: 2026-10-25
product: finfluencer.trade
component: gor_dagster
status: active
tags:
  - speaker-identification
  - speaker-resolution
  - content-item
  - episode-title
  - data-quality
---

# Episode-Title Speaker Hint

## What it is

When a registered show (Market Call, The Compound and Friends, The Acquirers Podcast) names its guest in the episode title, that publisher spelling is captured as an authoritative hint. Speaker Identification (SI) uses it to identify the guest under the publisher's spelling, and speaker resolution prevents auto-creating duplicate finfluencers under misheard speech-to-text transcript spellings.

Wrapped **2026-10-25** — permanent architectural doc: [episode-title-speaker-hint.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/episode-title-speaker-hint.md).

## Upstream: Speaker Identification (Recursive SI)

- **Pattern Registry & Consensus (`gor_dagster/registries/episode_title_guests.py`):**
  - Show-specific extractors pull candidate guest names from `ContentItem.title` (excluding known hosts).
  - `consensus_spellings` merges one-off title typos across the show's history (e.g. `Brendan Caldwel` -> `Brendan Caldwell`).
  - Precedence rule: explicit `known_speakers/{episode}.json` files override title hints.
- **Bootstrap Hypothesis & Canonicalization (`gor_dagster/utils/recursive_si_orchestrator.py`):**
  - Title guests are added as hypotheses to the bootstrap priors prompt block.
  - Close/loose heard names are canonicalized to the title guest spelling (`title_renames`), preserving provenance.
  - Single-guest shows seed missing guest stubs (`title_seeded`).

## Downstream: Speaker Resolution Title Guard

- **Decision Logic (`gor_dagster/services/episode_title_guard.py`):**
  - `decide_title_guard` evaluates title hints when the scorer would `AUTO_CREATE` a speaker on registered episodes:
    - `USE_TITLE_NAME`: scores the title spelling. If matching an existing finfluencer, auto-resolves with the heard name added as an `is_misspelling = TRUE` alias in `FinfluencerNameVariant`. If no holder, auto-creates under the title spelling with the heard name as an alias.
    - `QUEUE_MISMATCH`: single-guest show picker who does not match the title guest queues for curator review with code `TITLE_GUEST_MISMATCH`.
    - `QUEUE_AMBIGUOUS`: near multiple title guests queues with code `TITLE_GUEST_AMBIGUOUS`.
- **Dashboard & Tooling:**
  - `pipeline_dashboard`: curation pending table shows title notes directly in cell rows.
  - `scripts/audit_title_guest_spellings.py`: read-only audit script with `--yaml` to generate draft manifests for `scripts/correct_finfluencer_name_spellings.py`.
  - `scripts/check_episode_title_hints.py`: daily production verification check script.

## Key Rules & Invariants

1. **Unregistered shows untouched:** Shows not in `SHOW_TITLE_GUESTS` never run extraction, prompt injection, renames, seeds, or resolver overrides.
2. **No DDL & no extra LLM calls:** All metadata sits in existing string/JSON columns (`candidates`, `match_method = 'episode_title'`, `last_failure_reason_code`).
3. **Misspelling preservation:** Misheard STT names are stored as `is_misspelling = TRUE` aliases so future mishearings continue to match.

## Verification

- Over 300+ episodes processed in production under the title hint pipeline (2,734 resolved predictions, 209 pending).
- Upstream bootstrap canonicalization eliminated downstream spelling drift on new episodes (e.g. `Richard Orrell`, `Bruce Campbell`, `Keith Richards`, `Gordon Reid` all resolved cleanly).
- 7 new Market Call finfluencers auto-created since launch all carry the exact episode-title spelling (0 wrong).
- 23 historical misspelled finfluencers cleanly corrected via hand-reviewed manifests and `correct_finfluencer_name_spellings.py`.
