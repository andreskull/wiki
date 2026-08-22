---
type: project
title: "gor-blog"
product: finfluencer-trade
project: gor-blog
created: 2026-04-06
updated: 2026-08-19
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
- **Newsletter / Kit:** `api/newsletter/` — `send.py` (sitemap digest), ConvertKit client, `import_registered_users.py` (Supabase → `GOR_NEWSLETTER_SUBS`), custom one-off uploaders (`send_may_announcement.py`, `send_august_announcement.py`). Wrap-ups: [`registered-users-kit-import.md`](file:///Users/andreskull/gor-blog/api/newsletter/registered-users-kit-import.md), [`cumulative-compare-launch.md`](file:///Users/andreskull/gor-blog/api/newsletter/cumulative-compare-launch.md); full ops in [`README.md`](file:///Users/andreskull/gor-blog/api/newsletter/README.md).

## Important structural note

`docs/` in this repo IS the MkDocs site source, not project documentation. There is no separate `docs/architecture/` — the published content is the output.

**Feature specs (requirements / design / tasks) live at `gor-blog/features/<feature-name>/`, NOT under `docs/`.** This is a **gor-blog-specific deviation** from the standard `docs/features/` convention used in `gor_dagster`, `finfluencer-tracker`, and `rattaproff` — putting feature specs under `docs/` would expose them on the public MkDocs site (`finfluencers.trade`). All in-progress feature work for this repo must use `features/` at repo root. Established 2026-05-05. After wrapup, durable notes move to [`WIKI.md`](file:///Users/andreskull/gor-blog/WIKI.md) (and wiki) and the feature folder is removed.

```
gor-blog/
├── docs/                    ← MkDocs site source (PUBLIC)
│   ├── blog/posts/          ← published articles
│   ├── assets/              ← images, fonts, logos
│   ├── directory/           ← finfluencer directory content
│   └── stylesheets/         ← theme overrides
├── features/                ← feature specs (PRIVATE — outside MkDocs build)
│   └── <feature-name>/
│       ├── requirements.md
│       ├── design.md
│       └── tasks.md
├── research/                ← long-form research drafts (PRIVATE)
└── api/                     ← newsletter / Kit integration code
```

Internal implementation notes that are tightly coupled to specific code modules sit next to that code (e.g. `api/newsletter/registered-users-kit-import.md`, `api/newsletter/cumulative-compare-launch.md`), not under `features/`.

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

**[`growth_plan.md`](file:///Users/andreskull/gor-blog/growth_plan.md)** (repo root) — Finfluencers.trade growth plan, competitive notes, and pre-launch actions. Central index: [[products/finfluencer-trade]] § Planning and strategy. The *Cramer Paper Promotion Plan* phased checklist lives there; the **live cross-channel campaign log** (permalinks, metrics, variants) is [`research/cramer/promotion/cramer-study-launch-campaign.md`](file:///Users/andreskull/gor-blog/research/cramer/promotion/cramer-study-launch-campaign.md) *(supersedes the old `linkedin_promotion_plan.md` path)*.

## Required elements on every blog post

Every blog post in `docs/blog/posts/` must include the CTAs codified in [[concepts/blog-post-cta-pattern]] before publishing — above-the-fold module, inline mid-article module, expanded end-of-post block (newsletter + product CTA), and internal product links throughout the body. The pattern was established 2026-05-05 after the Cramer launch revealed 92% article-to-product abandonment uniform across acquisition channels (GA4 Funnel exploration, May 4 2026). See `research/cramer/promotion/cramer-study-launch-campaign.md` § *Day 1 learnings + plan revision* for the source data.

## Current status

Live site with active publication. Platform-update post **2026-08-14**: *Cumulative Curves, Head-to-Head Compare, and Every Show We Track* (`https://finfluencers.trade/blog/2026/08/14/cumulative-performance-and-show-leaderboards/`). Kit broadcast `25439881` the same day; `last_newsletter_date` **2026-08-14**. Custom one-off senders live next to `send.py` — wrap-up [`cumulative-compare-launch.md`](file:///Users/andreskull/gor-blog/api/newsletter/cumulative-compare-launch.md). Directory operational — **Investing Unscripted** under Covered (`investing-unscripted`, **2026-07-27**). **Chit Chat Stocks** already has a directory entry (`data-key=chit-chat-stocks`); Covered `_profiles.json` mapping still **deferred** after pipeline onboarding **2026-08-02** ([[projects/gor_dagster]]). Every new pipeline show must revise the directory (playbook Requirement 12 — [[concepts/onboarding-new-podcast-source]]). **Newsletter:** footer form POSTs to **`/api/subscribe`** on gor-blog (Vercel serverless). When the blog is viewed via **`finfluencers.trade/blog/...`**, the landing app (**[[projects/finfluencer-tracker]]**) rewrites **`/api/subscribe`** to **`blog.finfluencers.trade`** so subscriptions work at apex. Registered app users can still be merged into Kit tag **`GOR_NEWSLETTER_SUBS`** via CLI import (see repo `api/newsletter/`). **CTA pattern** (hero + inline + end-of-post dual card + internal links): Cramer study plus the 2026-08-14 platform post. End cards are an equal-width green-border grid (`.ft-cta-endpost`). Raw HTML asset paths in posts must be root-absolute (`/assets/...`). CSS in `docs/stylesheets/extra.css` — see [`WIKI.md`](file:///Users/andreskull/gor-blog/WIKI.md) § *Blog post CTA pattern*. Cramer working paper is public on **SSRN** and **GitHub** (see [[projects/cramer-mad-money-research]]); private `research/cramer/` holds export/spec/admin files only.

## Related pages

- [[products/finfluencer-trade]]
- [[projects/gor_dagster]]
- [[projects/cramer-mad-money-research]]
- [[projects/finfluencer-tracker]]
- [[concepts/blog-post-cta-pattern]]
