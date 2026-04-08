---
type: project
title: "gor-blog"
product: finfluencer-trade
project: gor-blog
created: 2026-04-06
updated: 2026-04-07
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

## Research drafts (`research/`)

Long-form research that is **not** part of the public MkDocs tree lives under **`gor-blog/research/`** (e.g. **`research/cramer/`** for the Cramer methodology paper). Vault-worthy material must be **ingested**: copy to **`wiki/raw/papers/`** plus **`wiki/sources/YYYY-MM-DD-slug.md`** (see [[synthesis/lint-gor-blog-internal-papers-2026-04-07]] — rule updated when files moved out of `_internal/papers/`). Example: [[sources/2026-04-07-cramer-mad-money-performance-methodology]].

## Where research work lands

New articles about finfluencers or the platform are written directly as posts in `docs/blog/posts/`. Working drafts may live under **`research/`** or the wiki **`raw/`** folder; published posts are content outputs in `docs/blog/posts/`.

## Current status

Live site with active publication. 14 posts published. Directory operational.

## Related pages

- [[products/finfluencer-trade]]
- [[projects/gor_dagster]]
- [[projects/finfluencer-tracker]]
