---
type: synthesis
title: "Lint — finfluencer.trade (2026-04-06)"
product: finfluencer-trade
project: null
created: 2026-04-06
updated: 2026-04-06
tags: [lint, wiki-health, finfluencer-trade]
---

# Lint — finfluencer.trade (2026-04-06)

Scoped health check for wiki pages tied to **finfluencer.trade**: product page, three project pages, linked concepts/entities, and cross-references to repos.

## Resolved during this pass

| Issue | Resolution |
|-------|------------|
| [[entities/bigquery]] dataset naming | Corrected to **`dagster_prod`** + **`dagster_shared`** per live `bq ls` on `gurus-on-record` |
| [[entities/bigquery]] linked to missing [[entities/gcs]] | Added minimal [[entities/gcs]] page |
| [[projects/finfluencer-tracker]] follow-up | **`docs/architecture/`** added (`README`, `system-overview`, `data-layer`); **`WIKI.md`** updated; [[projects/finfluencer-tracker]] expanded |
| [[projects/gor-blog]] hosting | **Vercel** (confirmed in repo `README.md`, `package.json`, `growth_plan.md`) — updated wiki project page |

## Index and orphan check

- **index.md:** All finfluencer.trade rows (`products/finfluencer-trade`, `gor_dagster`, `gor-blog`, `finfluencer-tracker`) present and consistent.
- **Orphans:** No stray finfluencer-trade pages outside the catalog.

## Repo cross-checks

| Claim | Evidence |
|-------|----------|
| 14 published posts in `gor-blog` | 14 `*.md` files under `docs/blog/posts/` |
| 7 active features in `gor_dagster` | Matches `WIKI.md` Active features list |
| Blog post filenames cited on [[projects/gor-blog]] | Sample filenames match repo |

## Remaining actions (priority)

### Low

1. **Overview freshness:** After the next feature **wrapup** in `gor_dagster`, refresh [[wiki/overview]] “Active development” and completed-feature bullets.

## Related pages

- [[products/finfluencer-trade]]
- [[projects/gor_dagster]]
- [[projects/gor-blog]]
- [[projects/finfluencer-tracker]]
- [[entities/bigquery]]
- [[entities/gcs]]
