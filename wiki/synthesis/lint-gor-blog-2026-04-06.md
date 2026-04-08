---
type: synthesis
title: "Lint — gor-blog (2026-04-06)"
product: finfluencer-trade
project: gor-blog
created: 2026-04-06
updated: 2026-04-06
tags: [lint, wiki-health, gor-blog, mkdocs]
---

# Lint — gor-blog (2026-04-06)

Scoped health check for **gor-blog** (`wiki/projects/gor-blog`), repo **`WIKI.md`**, and vault indexing rules.

## Resolved during this pass

| Issue | Resolution |
|-------|------------|
| Post count **14** on [[projects/gor-blog]] | Verified: 14 `*.md` files in `gor-blog/docs/blog/posts/` |
| **`docs/features/`** described as scaffold | Verified: directory exists, **empty** |
| Repo **`WIKI.md`** vs vault rules | Stated posts were “NOT indexed” — **wrong**. Vault indexes `docs/blog/posts/` as **published content outputs** (see `wiki/CLAUDE.md`). **Updated `gor-blog/WIKI.md`** to match |
| [[index.md]] project row | **finfluencer-tracker** still said “Sparse docs — stub”; updated to reflect **`docs/architecture/`** + expanded wiki page |

## Index and orphan check

- **index.md:** [[wiki/projects/gor-blog]] present.
- **Orphans:** None for gor-blog.

## Repo cross-checks

| Claim | Evidence |
|-------|----------|
| MkDocs + Material | `mkdocs.yml`, `requirements.txt` |
| Vercel hosting | `vercel.json`, `README.md`, `package.json` |
| No `docs/architecture/` | Not present — correct for this repo (site = product) |

## Remaining actions (priority)

### Low

1. **Overview freshness:** After the next **wrapup** in `gor_dagster`, refresh [[wiki/overview]] “Active development” if feature list changes.

## Related pages

- [[projects/gor-blog]]
- [[products/finfluencer-trade]]
- [[projects/gor_dagster]]
