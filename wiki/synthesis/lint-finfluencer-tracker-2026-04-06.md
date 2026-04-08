---
type: synthesis
title: "Lint — finfluencer-tracker (2026-04-06)"
product: finfluencer-trade
project: finfluencer-tracker
created: 2026-04-06
updated: 2026-04-06
tags: [lint, wiki-health, finfluencer-tracker]
---

# Lint — finfluencer-tracker (2026-04-06)

Scoped check for **finfluencer-tracker**: [[wiki/projects/finfluencer-tracker]], repo **`WIKI.md`**, **`docs/architecture/`**, and vault indexing rules.

## Resolved during this pass

| Issue | Resolution |
|-------|------------|
| Wiki indexing scope in repo **`WIKI.md`** | Stated only **`docs/architecture/`** — vault rule is **`WIKI.md` + all `docs/` except `docs/features/`** (see `wiki/CLAUDE.md`). **Updated `finfluencer-tracker/WIKI.md`** and aligned [[wiki/projects/finfluencer-tracker]] + [[wiki/overview]] |
| [[wiki/projects/finfluencer-tracker]] intro line | Short blurb updated so it matches the expanded architecture description |
| Cross-reference **`gor_dagster`** `supabase-sync-architecture.md` | File exists at `gor_dagster/docs/architecture/supabase-sync-architecture.md` |
| Architecture files in repo | Present: `docs/architecture/README.md`, `system-overview.md`, `data-layer.md` |

## Index and orphan check

- **index.md:** [[wiki/projects/finfluencer-tracker]] row already “Active — `docs/architecture/` in repo” — left as-is (accurate highlight).

## Remaining actions (priority)

### Low

1. Re-run **`wiki sync finfluencer-tracker`** after future **`docs/`** edits so the Obsidian page stays current.

## Related pages

- [[products/finfluencer-trade]]
- [[projects/gor_dagster]]
- [[projects/gor-blog]]
