---
type: concept
title: "Google Ads / GA4 conversion tracking (finfluencers.trade)"
product: finfluencer-trade
project: null
created: 2026-08-04
updated: 2026-08-11
tags: [google-ads, ga4, analytics, conversion-tracking, marketing]
---

# Google Ads / GA4 conversion tracking (finfluencers.trade)

How paid-acquisition measurement is wired for finfluencers.trade.

## Topology (verified end-to-end 2026-08-04)

One Google tag on the site, feeding two destinations.

```
Google tag "Finfluencers.Trade"
  Tag IDs:  G-BLE3H05Q5T, GT-TWMLQVCG, AW-18322362149, GT-K8HQDHPS
  → GA4 property "Finfluencers.Trade", 485294334   (added 2025-11-04)
  → Google Ads account "Finfluencers.Trade", AW-18322362149  (added 2026-07-14)

GA4:  account a16171064 / property 485294334
      web stream "Finfluencers.Trade", stream ID 10516700775
      URL https://finfluencers.trade
      measurement ID G-BLE3H05Q5T
      connected site tags: 0        status: data flowing

Google Ads:  account 635-115-7649 (ocid=8404411852)
```

`G-BLE3H05Q5T` **is** the measurement ID of GA4 property 485294334. The tag and the property are the two ends of one pipe, not two systems. **Trust IDs, not display names.**

*Historical footnote: the tag carried the legacy display name "Finfluencers.Bet" from the pre-rename domain until 2026-08-04, which repeatedly caused people and LLMs to conclude there were two tracking setups. Renamed; the confusion should not recur.*

## The live trap

Google Ads' conversion **data-source picker** lists these side by side as if they were alternatives:

| Shown as | Marked | What it actually is |
|---|---|---|
| the tag, by display name | *Saidile paigaldatud* (installed on site) | the Google tag |
| Finfluencers.Trade — `485294334` | *Selle kontoga lingitud* (linked to this account) | the GA4 property the same tag feeds |

**Both must be selected.** Until 2026-08-04 only the tag was, which is why no GA4 key event could reach Google Ads at all — it looked like sync latency, but the property was simply never enabled as a conversion data source.

Related: a page's HTML may reference only `G-BLE3H05Q5T` while `AW-18322362149` still exists as a tag destination. A statement about page source is not a statement about tag configuration. (`AuthCallback.tsx` additionally fires a native Ads conversion directly, so the AW- path is real in code too.)

## Conversion actions (as of 2026-08-04)

| Action | Source | Role |
|---|---|---|
| Registreerumine (sign_up) | Website, native custom event | **Primary** — the bidding signal |
| Registreerumine (Lehe laadimine finfluencers.trade/signup) | Website, URL rule | Secondary — form-arrival observation only |

Both Lehevaatamine actions were archived 2026-08-04.

### The correction that matters

Until 2026-08-04 the **primary** conversion was the page-load action, whose rule is *"someone visits a page starting with finfluencers.trade/signup"* — it counted **arrivals at the signup form**, not completed registrations. Smart Bidding was optimising for form arrivals and every historical cost-per-conversion figure used the wrong denominator.

The true signal is `AW-18322362149/6l99COj_3NUcEKWe5KBE`, fired from `src/pages/AuthCallback.tsx` only after a real Supabase session, deduped per user via localStorage plus a 5-minute `created_at` window. It is now primary. Note it does **not** depend on GA4 key-event status — it is a native Ads tag conversion.

Expect reported conversions to fall and cost-per-conversion to rise for several weeks. That is the numbers becoming honest, not performance degrading.

**Standing rule:** a conversion action whose rule is a URL/page-load match, or an event that fires on essentially every pageview, must never be primary.

### `view_page` — imported and retired the same day

A site-wide pageview custom event was briefly made a GA4 key event and imported, on the assumption the account had almost no usable signal. An audit of `finfluencer-tracker` the same day disproved that: eleven GA4 events already fire from `src/lib/analytics.ts` and its domain modules; they were simply never marked as key events, so Google Ads could not see them.

It was retired because it collapsed GA4's key-events metric into a pageview count and produced an Ads action that was primary-within-goal with the secondary toggle greyed out. **Confirmed mechanism: un-keying a GA4 event breaks its imported Google Ads conversion action.** The GA4 event itself was kept — harmless as an ordinary event, still useful for segmentation. Only key-event status caused harm.

The exercise did earn one thing: it proved the GA4 → Ads chain works once the property is enabled as a data source.

## Measurement plan

**Update 2026-08-11 — the "missing entirely" half of this section is now shipped and live in production.** `finfluencer-tracker`'s conversion measurement plan (Phases 1–3) instrumented `purchase`, `paywall_hit`, `profile_viewed`, `leaderboard_engaged`, `signals_engaged` and `onboarding_completed`. `purchase`, `paywall_hit`, `profile_viewed`, `leaderboard_engaged` and `signals_engaged` were confirmed firing on production 2026-08-11 (real Stripe purchase for `purchase`; GA4 Realtime for the rest). `onboarding_completed` is implemented but not yet confirmed live — it fires once per user, so confirming it needs a not-yet-onboarded account.

The app now emits 15 custom events total (16 counting `sign_up`, which predates this plan and is already a GA4 key event / Ads primary). **None of the 15 are exposed to Ads yet** — that is the entire remaining scope, tracked as Phase 4 in `docs/features/conversion-measurement-plan/tasks.md`:

`purchase`, `paywall_hit`, `profile_viewed`, `leaderboard_engaged`, `signals_engaged`, `onboarding_completed`, `comparison_created`, `begin_checkout`, `performance_chart_viewed`, `performance_chart_period_changed`, `animation_played`, `export_requested`, `export_completed`, `share_completed`, `watermark_link_tapped`.

**Progress so far on Phase 4:**
- `newsletter_signup` removed as a GA4 key event 2026-08-11 (Task 4.1) — it belonged to `gor-blog`, not this app, and had never fired here; see the `view_page` postmortem above for the same class of mistake. GA4 key events are now exactly `sign_up` (primary in Ads) — the 15 above are un-keyed and unimported.
- Task 4.2 onward (key-event marking → Ads secondary import → conversion values → bid strategy → primary promotions) is a sequential checklist in `tasks.md`, gated in the order D1 there specifies — **do not promote anything to primary as a side effect of an earlier task.** `sign_up`, and later `purchase` and `comparison_created`, are the only conversions that should ever be primary.
- **2026-08-11 note if you are picking this up fresh**: `docs/features/conversion-measurement-plan/requirements.md`'s "GA4 configuration" and "Google Ads configuration" acceptance criteria previously listed two different, both-incomplete subsets of these 15 events (missing `signals_engaged` from both, and missing 5 others from one or the other). That was corrected the same day — `requirements.md` and `tasks.md` Task 4.2 now agree and are both authoritative. If you find them disagreeing again, that itself is worth fixing before proceeding, not just picking one.

Specified in `finfluencer-tracker` → `docs/features/conversion-measurement-plan/`.

## Lessons that generalise

- When a UI shows a **name**, verify the underlying **ID**. Names go stale after rebrands; IDs do not.
- When replacing a broken tracking object, read the **original's actual scope** first. Never guess narrower by default.
- Before concluding a signal is missing, check whether the product already emits it and is simply not wired up.

## Related pages

- [[products/finfluencer-trade]]
- [[projects/finfluencer-tracker]]
- [[concepts/subscription-entitlement-ssot]]
