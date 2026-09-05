---
type: entity
title: "BigQuery"
product: null
project: null
created: 2026-04-06
updated: 2026-09-02
tags: [bigquery, gcp, data-warehouse, sql]
---

# BigQuery

Google Cloud's serverless data warehouse. Used across multiple projects.

## How it's used

- **finfluencer.trade / gor_dagster (project `gurus-on-record`):** Core objects are split by role across three datasets:
  - **`dagster_prod`** — RSS / episode catalogue only: `ContentSource`, `ContentItem`, `ContentSourceTimingConfig`. This is what the default `bigquery_resource` uses via `BIGQUERY_DATASET_ID` (see `gor_dagster/env.example`).
  - **`dagster_shared`** — Main pipeline warehouse: `Finfluencer`, `FinancialInstrument`, `PotentialPrediction`, `ActionableSignal` (view), `stt_operations`, `stt_speaker_attributions`, batch job tables, facts extraction, and dozens of other tables/views. The `app_bq_resource` in `definitions.py` is pinned to `dagster_shared` (not env-driven) for assets that must always hit that dataset.
  - **`dagster_prices`** — Price and performance warehouse: `PriceHistory`, `TradingCalendar`, `SignalPerformance`, and performance aggregate views. `SignalPerformance` lives here, not in `dagster_shared`.
- **rattaproff:** Indexing backlog governance — URL action queue with status metadata.
- **finfluencer.trade / finfluencer-tracker (GA4 export, project `gurus-on-record`):** One-directional GA4 → BigQuery raw-event export, linked 2026-09-02 for Web Vitals field verification ([[concepts/core-web-vitals-mobile]], [[decisions/ga4-instrumentation-registration-2026-09]]). Daily export creates `events_YYYYMMDD` (+ `events_intraday_YYYYMMDD`) in dataset `analytics_485294334`, one row per event, plus `pseudonymous_users_YYYYMMDD`. Data location **US multi-region** (GA4 default, kept — operational/performance telemetry, not user data). Separate from, and does not touch, the `dagster_*` datasets below.

## Key patterns (gor_dagster)

- Use **load jobs** (not streaming inserts) for data that may need modification — streaming buffer prevents row deletion
- Tables are **partitioned by creation date** and **clustered** by source_id / content_type
- `ActionableSignal` is a VIEW, not a table — delete from `PotentialPrediction`
- **`PotentialPrediction`** (prod `dagster_shared`) has **`require_partition_filter = TRUE`** — every query/DML must filter on partition column **`created_at`**. Program write-up: [bigquery-cost-optimization.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/bigquery-cost-optimization.md).
- JSON columns used for extensible metadata

## Projects using it

- [[projects/gor_dagster]]
- [[projects/rattaproff]]
- [[projects/finfluencer-tracker]] (GA4 export only, separate from the gor_dagster pipeline datasets)

## Related pages

- [[entities/dagster]]
- [[entities/gcs]]
- [[concepts/actionable-signal]]
- [[concepts/core-web-vitals-mobile]]
- [[decisions/ga4-instrumentation-registration-2026-09]]
- [[projects/finfluencer-tracker]]
