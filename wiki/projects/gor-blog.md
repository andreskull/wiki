---
type: project
title: "gor-blog"
product: finfluencer-trade
project: gor-blog
created: 2026-04-06
updated: 2026-04-27
tags: [blog, mkdocs, content, finfluencer, research, articles]
---

# gor-blog

The public-facing site and content layer of [[products/finfluencer-trade]]. An MkDocs-based blog and finfluencer directory where research, analysis, and platform updates are published.

## Product

[[products/finfluencer-trade]]

## Purpose and role

The public voice of the platform. Publishes research on finfluencer prediction quality, platform development updates, methodology pieces, and maintains a browsable directory of tracked influencers. This is where research work products (articles, analyses) live — not in the wiki.

## Tech stack

- **Site generator:** MkDocs with Material theme
- **Content format:** Markdown (blog posts, directory)
- **Hosting:** [Vercel](https://vercel.com/) — static build (`mkdocs build` / `vercel-build`); env and domains documented in the repo `README.md`
- **Assets:** Fonts, logos, images in `docs/assets/`

## Important structural note

`docs/` in this repo IS the MkDocs site source, not project documentation. There is no separate `docs/architecture/` or similar — the published content is the output.

```
docs/
├── blog/posts/     ← published articles (14 posts as of bootstrap)
├── assets/         ← images, fonts, logos
├── directory/      ← finfluencer directory content
└── features/       ← empty (spec-driven scaffold, unused)
```

## Published content (as of 2026-04-06)

14 blog posts covering:
- Finfluencer research and accountability (`the-finfluencer-mirage.md`, `understanding-finfluencer-impact-on-investors.md`)
- Content picks and academic paper reviews (`content-pick-finfluencers-academic-paper.md`, `critique-of-wallstreetbets-predictive-power-paper.md`)
- Platform/tech posts (`dagster-hybrid-gcp-deep-dive.md`, `how-i-apply-spec-driven-ai-coding.md`, `finding-optimal-quality-vs-cost-in-large-context-llm-tasks.md`)
- Product updates (`introducing-finfluencers-directory.md`)

## Research (`research/`)

Long-form research that is **not** part of the public MkDocs tree may live under **`gor-blog/research/`** until ingested to the vault.

**Cramer / *Mad Money* (2018–2024)** is split across two locations:

- **Public** — [[projects/cramer-mad-money-research]]: frozen CSV/Parquet, analysis-only scripts, figures, working paper (Markdown + PDF). **SSRN** [6643379](https://ssrn.com/abstract=6643379). [GitHub](https://github.com/andreskull/cramer-mad-money-research).
- **Private** — **`gor-blog/research/`** ([`README.md`](file:///Users/andreskull/gor-blog/research/README.md)) and **`research/cramer/`**: `README.md`, `SSRN_submission.md`, `research_plan.md`, `SPEC_*.md`, **`scripts/`** (BigQuery export, `reclassify_holds` with `enrich`/`extract`, Fama/phase prep). Superseded QQQ-era exploratory scripts were removed from the tree (2026-04); **git history** retains them. Outputs land in the public repo clone’s `data/`.

Vault methodology archive: [[sources/2026-04-07-cramer-mad-money-performance-methodology]] (`wiki/raw/papers/…`). For new long-form that should live in the vault, still **ingest** per [[synthesis/lint-gor-blog-internal-papers-2026-04-07]].

## Where research work lands

New articles about finfluencers or the platform are written directly as posts in `docs/blog/posts/`. Working drafts may live under **`research/`** or the wiki **`raw/`** folder; published posts are content outputs in `docs/blog/posts/`.

## Current status

Live site with active publication. 14+ posts published. Directory operational. Cramer working paper is public on **SSRN** and **GitHub** (see [[projects/cramer-mad-money-research]]); private `research/cramer/` holds export/spec/admin files only.

## Related pages

- [[products/finfluencer-trade]]
- [[projects/gor_dagster]]
- [[projects/cramer-mad-money-research]]
- [[projects/finfluencer-tracker]]
