---
type: synthesis
title: "Finfluencers.trade GTM / growth hub"
product: finfluencer-trade
project: null
created: 2026-09-18
updated: 2026-09-24
tags: [gtm, growth, marketing, ads, outreach, newsletter]
---

# Finfluencers.trade GTM / growth hub

**Living source of truth** for go-to-market and growth. Start here for priorities and where channel docs live.

Last reviewed: **2026-09-18** (Europe/Tallinn).

## How to use this page

| Need | Go here |
|---|---|
| Current priorities | **Priorities** below |
| What’s already running | **Live** below |
| Channel mechanics | Linked [[concepts/*]] pages |
| Campaign packs / Kit how-to | `gor-blog/` root markdown (see doc map) |
| Ad creatives / PMax assets | `finfluencer-tracker/marketing/` |
| Outreach runners | `gor_dagster` scripts + ops docs |
| Morning Ads review | Claude Scheduled: `ads-performance-morning-review` |

Update this page when priorities change, a channel ships or pauses, or a campaign pack is added. Keep durable “how it works” on concept pages; keep one-off campaign copy in repo files.

**Planning rule:** list **activities and priorities only** — no dated calendars or phased timeboxes.

## ICP (ideal customer profile)

**ICP** = who the product is for: the buyer/user we aim ads, content, and product at.

Current best signal (from Google Ads demographics): **higher-income, experienced investors** (~55–64 and top income bands outperform). They watch financial TV / long-form; LinkedIn + email fit better than short-form entertainment apps. Depth beats reach.

Market position: **accountability / track record vs S&P**, not trade tips. Creative claim = coverage and receipts, not alpha.

Compliance: do **not** auto-send performance outreach to employees of **registered** financial entities — [[decisions/exclude-regulated-finance-employees-from-outreach-2026-08]].

### Why TikTok/Shorts are weak for this ICP

TikTok (and most Shorts feeds) skew younger, entertainment-first, short attention. Our ad data so far points at older, higher-income investors who research and watch CNBC-style long-form — not swipe-for-fun short video. So TikTok is **out** until evidence says otherwise. **YouTube** (long-form) fits the ICP better but is parked until higher-leverage Ads/list/LinkedIn work is solid.

## Pipeline (Overheard ↔ Grok Bot) — locked 2026-09-18

- Blog CMS: MkDocs `gor-blog`, **Finfluencer Research** category
- Notion: thin **status rows only** (no requirements/design dumps)
- F5Bot Apps Script: **full rebuild** of `_internal/f5bot-automation/` (not patch)
- X stays in **Overheard firehose**
- Engagement accounts + which app data to cite: defer until drafting
- Intake packets: `gor-blog/_internal/pipeline-intake/`

## Live (already running — do not rebuild)

| Channel / surface | Notes / wiki |
|---|---|
| Google Ads Search + PMax | Conversions wired; PMax creatives V1+V4 live — [[concepts/google-ads-conversion-tracking]], [[concepts/google-ads-creative-assets]] |
| Reddit Ads | Conversions live; Traffic Max paused — [[concepts/reddit-ads-conversion-tracking]] |
| PeerPush | Campaign existed; **renewed / currently active** |
| F5Bot mention monitoring | Alerts by email (Reddit primary). Apps Script triage/drafts in `gor-blog/_internal/f5bot-automation/` — **built, unsatisfactory → rebuild** |
| Microsoft Clarity | Live — [[concepts/session-replay-analytics]] |
| Kit / ConvertKit newsletter | Live; signup/newsletter list grows over time; guide in `gor-blog` |
| Blog CTA pattern | Live — [[concepts/blog-post-cta-pattern]] |
| Cramer research TOFU | v1 shipped (blog/SSRN/GitHub). Promotion frozen 2026-09-24 — [`CONSERVATION.md`](file:///Users/andreskull/gor-blog/research/cramer/CONSERVATION.md) |
| LinkedIn outreach pipeline | Notion Follow→Connect→Intro→Respond; AI drafts; human send — [[concepts/linkedin-outreach]] |
| Public funnel / Explore nav | Leaderboard / Compare / Shows; Spectator→Trader mask |
| Post-signup onboarding | Skippable — [[concepts/post-signup-onboarding]] |
| CNBC IPO / SpaceX scoreboard | Live route; campaign pack in `gor-blog` |
| X / Twitter | Manual only today |

## My Plate

Human attention merge lives in Notion **My Plate** (not this priority list): https://app.notion.com/p/8c6b81e137dc4bd1b4a77277d6fd0c78 — see also [[synthesis/my-plate]].

## Priorities (locked 2026-09-19)

Order is the working queue. No timeline phases.

1. Ads conversion gates — optimize on meaningful on-site events, not vanity
2. Onboarding skip — reduce high skip rate (Clarity)
3. LinkedIn Affiliation backfill — regulated-employer gate
4. Customer-list → Google + Reddit audiences (recurring; other networks later after analysis)
5. Engaged-user nurture — segment first (longer/return visits), then emails
6. Meta Pixel + Meta ads setup
7. LinkedIn regular posting — finfluencer / accountability thought leadership
8. Rebuild F5Bot automation — full rebuild of `gor-blog/_internal/f5bot-automation/` (not patch)
9. X monitoring decision — stays in Overheard firehose for now; revisit source/manual later
10. Low-ticket PDF SKU — optional one-time paid report on site
11. YouTube — parked
12. TikTok / Shorts — out

**Dropped:** Cramer PDF lead magnet (gated PDF offer) — 2026-09-19.

**Monitor only:** PeerPush (renewed / live).

### Overheard / research–engage (separate lane)

Not part of the numbered GTM queue. Overheard scores digests → RESEARCH+ENGAGE packets live in `gor-blog/_internal/pipeline-intake/` (thin Notion status rows only). Publish/reply decisions are ad hoc; do not promote each packet into this priority list.


## Explicitly deprioritized / out of scope for now

- TikTok / Shorts
- Rebuilding live Google/Reddit Ads stacks, Clarity, Kit, Explore funnel from scratch
- Treating deleted July checklist / `growth_plan.md` as current
- PeerPush “launch” as a new task — **already renewed / live** (ops: keep an eye on it, don’t re-plan as greenfield)

## Doc map

| Artifact | Path | Role |
|---|---|---|
| **This hub** | `wiki/synthesis/gtm-growth.md` | Current GTM SSOT |
| Kit guide | [`gor-blog/convertkit-newsletter-guide.md`](file:///Users/andreskull/gor-blog/convertkit-newsletter-guide.md) | How to send |
| SpaceX / CNBC meme pack | [`gor-blog/spacexbets-meme-campaign-2026-07.md`](file:///Users/andreskull/gor-blog/spacexbets-meme-campaign-2026-07.md) | One-off campaign |
| Cramer v1 conservation | [`gor-blog/research/cramer/CONSERVATION.md`](file:///Users/andreskull/gor-blog/research/cramer/CONSERVATION.md) | What to keep before a re-analysis. Campaign log is frozen |
| PMax creative brief + assets | [`finfluencer-tracker/marketing/`](file:///Users/andreskull/finfluencer-tracker/marketing/) | Execution assets |
| Reddit ads notes | [`finfluencer-tracker/reddit-ads-call-brief-2026-08-17.md`](file:///Users/andreskull/finfluencer-tracker/reddit-ads-call-brief-2026-08-17.md) | Channel setup notes |
| LinkedIn outreach state | [`gor-blog/.linkedin-outreach-state.json`](file:///Users/andreskull/gor-blog/.linkedin-outreach-state.json) | Recent run snapshot |
| F5Bot automation (rebuild) | `gor-blog/_internal/f5bot-automation/` | Alert triage / draft replies |
| Pipeline / outreach ops | `gor_dagster` docs + scripts | Execution |
| App MVP / post-MVP product | `gor_dagster/docs/MVP_MASTER_PLAN.md`, `INCR_01_MASTER_PLAN.md` | Product scope (not GTM) |

## Channel ownership

| Channel | Primary surface | Cadence note |
|---|---|---|
| Google Ads | Tracker pixels + `gor_dagster` creative archive; morning-review skill | Review regularly; refresh creatives when strength drops |
| Reddit Ads | Tracker pixel | As needed |
| PeerPush | PeerPush campaign | Renewed / live — monitor |
| Meta Ads | To stand up; needs Pixel | After Pixel + creative |
| Reddit organic | F5Bot alerts → (rebuilt) drafts → human reply | When alerts fire |
| LinkedIn organic + outreach | Personal LinkedIn; Notion; `gor_dagster` pipeline | Pipeline + human send; regular posts |
| X / Twitter | Manual today | Decide monitor later |
| Newsletter | `gor-blog` Kit API | Nurture **engaged** segment; broadcast after meaningful ships |
| Blog / SEO | `gor-blog` | Research + platform updates |
| Clarity | Tracker | Engaged-user definition + onboarding review |

## Related pages

- [[products/finfluencer-trade]]
- [[concepts/google-ads-conversion-tracking]]
- [[concepts/google-ads-creative-assets]]
- [[concepts/reddit-ads-conversion-tracking]]
- [[concepts/linkedin-outreach]]
- [[concepts/session-replay-analytics]]
- [[concepts/post-signup-onboarding]]
- [[concepts/blog-post-cta-pattern]]
- [[decisions/exclude-regulated-finance-employees-from-outreach-2026-08]]
- [[projects/gor-blog]]
- [[projects/finfluencer-tracker]]
- [[projects/gor_dagster]]
