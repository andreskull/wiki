---
type: concept
title: "Sector ETF registry"
product: finfluencer-trade
project: gor_dagster
created: 2026-10-01
updated: 2026-10-01
tags: [instrument-resolution, sector, etf, bigquery, curation]
---

# Sector ETF registry

How a sector call ("buy energy", "I like semis") becomes a scored pick. Switched on in production on 2026-09-30.

## Definition

Facts extraction tags a sector call `mention_type = 'sector_theme'`. The resolver looks the spoken label up (`lower().strip()`) in `dagster_shared.SectorEtfRegistry`. That table maps exposure concepts and their spoken synonyms to an ETF, and the mention is linked to that ETF.

- **11 GICS concepts:** the sector's benchmark ETF from `sector_benchmarks.py` (VGT, VFH, VDE, VHT, VCR, VDC, VIS, VAW, VPU, VNQ, VOX), with pinned FIGIs. This gives one "tech ETF" across the platform.
- **16 other concepts:** market/size, semis, biotech, oil & gas, gold, silver, oil, bonds, EM, China and similar, with literals in the seed.

The ETF is the first candidate (canonical, then fallbacks) that exists and was tradable on the episode date.

Order of checks for a sector mention:
1. **Dismissed hash:** stays dismissed.
2. **Registry:** a known label resolves to its concept's ETF, even when a curator alias exists.
3. **Curator alias:** used only when the registry doesn't know the label.
4. **Human queue:** `REGISTRY_NO_MATCH`.

Nothing else in the cascade runs, so a generic label never lands on a company that shares the word. A spoken ETF ticker (XLE, GDX) bypasses the registry.

Curators add labels with **Map to sector concept** in the dashboard's curation queue. It adds the label as a synonym and resolves every pending row with that label. No alias and no rebrand log are written.

## Relevance to Andres's work

- **Accuracy:** sector calls were often linked through bad curator aliases: energy → UUUU, financials → TFC, healthcare → HR, semiconductors → NXPI. The registry fixes new mentions. Older rows keep their links; there was no remap.
- **Scoring:** unchanged. A sector call is benchmarked against SPY as `Cross-asset / ETF`.

## Projects using it

- [[projects/gor_dagster]] — resolver stage, seed, dashboard action. Permanent doc: `docs/architecture/features/sector-etf-registry.md`.

## Related concepts

- [[concepts/curation-learning]] — aliases and the poison-alias problem the registry sidesteps for sector labels
- [[concepts/post-cutoff-ipo-resolution]] — the first-tradable gate the ETF choice also respects
- [[concepts/resolution-pipeline-efficiency]]
