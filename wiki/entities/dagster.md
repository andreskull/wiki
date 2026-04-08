---
type: entity
title: "Dagster"
product: finfluencer-trade
project: gor_dagster
created: 2026-04-06
updated: 2026-04-06
tags: [dagster, orchestration, pipeline, assets, gcp]
---

# Dagster

Data orchestration framework. The backbone of [[projects/gor_dagster]].

## What it is

Open-source data orchestration platform. Organises data pipelines as typed, dependency-aware "assets" rather than tasks. Provides a UI for monitoring, materialization, and debugging.

## How it's used

- **Version:** 1.10.19 (pinned — all Dagster packages exact versions)
- **Deployment:** Dagster Cloud hybrid — cloud orchestration plane + self-hosted GCE VM agent running Docker
- **Development:** `dagster dev` for local UI
- **CI/CD:** GitHub Actions → Docker build → GCE VM deployment

## Key constraints

- All Dagster packages must be upgraded together (dagster, dagster-cloud, dagster-gcp, dagster-postgres)
- Default executor set to `max_concurrent: 2` to prevent GCE VM resource contention
- DNS resolution fails in multiprocess executor child processes → use `in_process_executor` for affected jobs
- Staging and production share the same GCP dataset (no separate staging)

## Projects using it

- [[projects/gor_dagster]] — core use

## Related pages

- [[entities/bigquery]]
- [[entities/gcs]]
- [[projects/gor_dagster]]
