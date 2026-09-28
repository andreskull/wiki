---
type: concept
title: "Post-cutoff IPO resolution and ticker tenancy"
product: finfluencer-trade
project: gor_dagster
created: 2026-09-28
updated: 2026-09-28
tags: [instrument-resolution, ipo, ticker-tenancy, bigquery, facts-extraction]
---

# Post-cutoff IPO resolution and ticker tenancy

## Definition

Facts extraction does not decide whether a company is public or which ticker it has. The model emits the spoken name with an optional ticker. Resolution decides tradability from the first EOD price bar, and, when a symbol has been reused, from the instrument that owned that ticker on the episode date.

A ticker is a slot. `BEAT` was BioTelemetry until 2021 and is HeartBeam after that. `v_ticker_tenancy` derives non-overlapping intervals from `FinancialInstrument.first_tradable_date` (NULL sorts first and means earliest owner). The end of an interval is the next owner's start.

## Relevance to Andres's work

Post-cutoff listings (SpaceX and peers) were silent signal loss: the frozen model dropped them or invented a ticker. Recycled symbols attached historical mentions to the current owner. Both are corrected in production. Returns for a delisted former owner stay empty — historical prices for those identities are not licensed.

Do not fill `first_tradable_date` on a shared ticker from EODHD `{TICKER}.US`. That series belongs to the current holder and would overwrite the NULL that keeps the former owner earliest.

## Projects using it

- [[projects/gor_dagster]] — `instrument_resolution.py`, `v_ticker_tenancy`, recursive FE prompts
- [[projects/finfluencer-tracker]] — displays the corrected `ActionableSignal` attribution; no Supabase schema change

## Related concepts

- [[concepts/actionable-signal]] — a pre-listing mention never becomes a signal
- [[concepts/curation-learning]] — unique-ticker bar and promotion sit on this cascade
- [[concepts/resolution-pipeline-efficiency]] — alias-on-resolve and re-attempt
- [[concepts/signal-performance]] — corrected historical signals can stay performance-NULL

## Sources

- [post-cutoff-ipo-resolution.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/post-cutoff-ipo-resolution.md)
