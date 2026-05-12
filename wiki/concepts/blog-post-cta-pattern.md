---
type: concept
title: "Blog post CTA pattern"
product: finfluencer-trade
project: gor-blog
created: 2026-05-05
updated: 2026-05-06
tags: [blog, cta, conversion, funnel, growth, mkdocs]
---

# Blog post CTA pattern

Standard CTA placement and style for every blog post in [[projects/gor-blog]]. Established 2026-05-05 after the Cramer launch revealed that 92% of article visitors abandoned the site without progressing to any product page (verified via GA4 Funnel exploration).

## Why this exists

The Cramer post launch on 2026-05-04 produced 25 article landings. Funnel exploration with `Page referrer` as breakdown showed:

| Page referrer | Step 1 visitors | Progressed to a non-article page | Completion |
|---|---:|---:|---:|
| https://www.reddit.com/ | 13 | 0 | 0% |
| direct (no referrer) | 6 | 1 | 16.7% |
| https://out.reddit.com/ | 2 | 1 | 50% (N=2) |
| https://www.linkedin.com/ | 2 | 0 | 0% |
| https://www.facebook.com/ | 1 | 0 | 0% |

The 92% abandonment was uniform across channels. Even Reddit — the strongest acquisition channel — had 0 of 13 desktop visitors progress. No amount of channel tuning solves this; the article itself needs clear next-step CTAs. Source: `gor-blog/research/cramer/promotion/cramer-study-launch-campaign.md` § *Day 1 learnings + plan revision*.

## Required elements (every new post)

Every blog post in `gor-blog/docs/blog/posts/` must include items 1–4 before publishing:

1. **Above-the-fold module.** A small, visually distinct box just below the title or intro paragraph, linking to the most relevant product page (specific finfluencer profile, signals view, or leaderboard). Visible without scrolling on both desktop and mobile. Suggested copy varies by post type — e.g. *"→ See live signals from this finfluencer"*, *"→ Track this finfluencer on the leaderboard"*.

2. **Inline mid-article module.** A callout box after the TL;DR or opening conclusions. Different copy from the above-fold module to avoid repetition. Example: *"Want to see signals like these as they happen? Browse the live leaderboard →"*.

3. **Expanded end-of-post block.** Alongside the existing newsletter subscribe form, add a second CTA linking to the live product (leaderboard or relevant profile). Two CTAs side-by-side; newsletter remains primary.

4. **Internal product links in the body.** Wherever the post mentions a specific finfluencer, signal type, or data slice, link to the corresponding live page (`/finfluencer/<slug>`, `/signals?slice=...`, `/leaderboard`, `/methodology`). No UTM tags on internal links.

## Optional / template-level (set once across all posts)

5. **Sticky element.** A persistent bottom bar or sidebar visible during scroll: *"Track 200+ finfluencers →"*. Should be dismissible. Implement once in the MkDocs theme; applies to every post automatically thereafter.

6. **Author/sidebar product CTA.** Extend the existing Andres Kull author block with a small product link: *"→ Track signals on finfluencers.trade"*.

## Implementation reference (gor-blog)

First complete application: **May 2026** — [`jim-cramer-stock-picks-study.md`](file:///Users/andreskull/gor-blog/docs/blog/posts/jim-cramer-stock-picks-study.md) (live: `https://finfluencers.trade/blog/2026/04/27/what-i-learned-from-16701-jim-cramer-stock-picks/`). Shared styles: **`docs/stylesheets/extra.css`** — **`.ft-cta-hero`**, **`.ft-cta-hero__copy`**, **`.ft-cta-hero__button`**, **`.ft-cta-inline`**. End-of-post block on that post uses the newsletter card plus a secondary leaderboard CTA (per-post HTML until a theme-level rollout). Durable notes: [`WIKI.md`](file:///Users/andreskull/gor-blog/WIKI.md) § *Blog post CTA pattern*.

## Style guidelines

- CTAs match the site's dark-mode palette and don't feel intrusive.
- Each post selects the *most relevant* target page for items 1, 2, and 3 — do not always link to the same page. A finfluencer-specific post links to that finfluencer's profile; a methodology post links to `/methodology`; a research post links to the relevant signals or leaderboard view.
- UTM tags are not required for internal links (same domain).
- Copy is calm and evidence-first to match site voice — no hype, no urgency words.

## Enforcement

When a new blog post is drafted, the writer (or LLM assistant) opens this page and confirms items 1–4 are present before publishing. Items 5 and 6 are template-level — set once in the MkDocs theme and inherited by every post.

## Related pages

- [[projects/gor-blog]]
- [[products/finfluencer-trade]]
- [[projects/cramer-mad-money-research]]

## Sources

- GA4 Funnel exploration "Cramer blog funnel", May 4 2026 (Page referrer breakdown).
- `gor-blog/research/cramer/promotion/cramer-study-launch-campaign.md` § *Day 1 learnings + plan revision*.
