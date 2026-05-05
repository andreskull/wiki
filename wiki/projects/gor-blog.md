---
type: project
title: "gor-blog"
product: finfluencer-trade
project: gor-blog
created: 2026-04-06
updated: 2026-05-05
tags: [blog, mkdocs, content, finfluencer, research, articles, newsletter, convertkit]
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
- **Newsletter / Kit:** `api/newsletter/` — `send.py`, ConvertKit client, `import_registered_users.py` (Supabase → same tag as footer, `GOR_NEWSLETTER_SUBS`). Wrap-up: [`registered-users-kit-import.md`](file:///Users/andreskull/gor-blog/api/newsletter/registered-users-kit-import.md); full ops in [`README.md`](file:///Users/andreskull/gor-blog/api/newsletter/README.md).

## Important structural note

`docs/` in this repo IS the MkDocs site source, not project documentation. There is no separate `docs/architecture/` — the published content is the output. In-progress feature specs are not kept under `docs/` after wrapup; internal notes sit next to code (e.g. `api/newsletter/registered-users-kit-import.md`).

```
docs/
├── blog/posts/     ← published articles
├── assets/         ← images, fonts, logos
├── directory/      ← finfluencer directory content
└── (no docs/features/ — removed after newsletter-import wrapup 2026-04)
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

## Growth and product strategy

**[`growth_plan.md`](file:///Users/andreskull/gor-blog/growth_plan.md)** (repo root) — Finfluencers.trade growth plan, competitive notes, and pre-launch actions. Central index: [[products/finfluencer-trade]] § Planning and strategy.

## Required elements on every blog post

Every blog post in `docs/blog/posts/` must include the CTAs codified in [[concepts/blog-post-cta-pattern]] before publishing — above-the-fold module, inline mid-article module, expanded end-of-post block (newsletter + product CTA), and internal product links throughout the body. The pattern was established 2026-05-05 after the Cramer launch revealed 92% article-to-product abandonment uniform across acquisition channels (GA4 Funnel exploration, May 4 2026). See `research/cramer/promotion/linkedin_promotion_plan.md` § *Day 1 learnings + plan revision* for the source data.

## Current status

Live site with active publication. 14+ posts published. Directory operational. **Newsletter:** registered finfluencer-tracker users can be merged into Kit tag **`GOR_NEWSLETTER_SUBS`** via CLI import (see repo `api/newsletter/`); template and Gmail dark-mode guidance in `convertkit_template_final.html` / README. Cramer working paper is public on **SSRN** and **GitHub** (see [[projects/cramer-mad-money-research]]); private `research/cramer/` holds export/spec/admin files only.

## Related pages

- [[products/finfluencer-trade]]
- [[projects/gor_dagster]]
- [[projects/cramer-mad-money-research]]
- [[projects/finfluencer-tracker]]
- [[concepts/blog-post-cta-pattern]]
