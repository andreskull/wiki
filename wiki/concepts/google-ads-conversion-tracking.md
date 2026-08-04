---
type: concept
title: "Google Ads / GA4 conversion tracking (finfluencers.trade)"
product: finfluencer-trade
project: null
created: 2026-08-04
updated: 2026-08-04
tags: [google-ads, ga4, analytics, conversion-tracking, marketing]
---

# Google Ads / GA4 conversion tracking (finfluencers.trade)

How paid-acquisition measurement is wired for finfluencers.trade, recorded because the naming in this setup has repeatedly caused people (and LLMs) to reach the wrong conclusion.

## The one rule

**There is exactly ONE Google tag on the site. Trust IDs, never display names.**

The old domain was `finfluencers.bet`. It was renamed to `finfluencers.trade`, but the Google tag kept the legacy display name "Finfluencers.Bet" until **2026-08-04**, when it was renamed to "Finfluencers.Trade". Every "we seem to have two tracking systems / a tag is missing / a GA4 property is orphaned" episode traces back to that stale label.

## Verified topology (end-to-end, 2026-08-04)

```
Google tag  "Finfluencers.Trade"   (display name was "Finfluencers.Bet" until 2026-08-04)
  Tag IDs:  G-BLE3H05Q5T, GT-TWMLQVCG, AW-18322362149, GT-K8HQDHPS
  Destination 1  ->  GA4 property "Finfluencers.Trade", 485294334   (added 2025-11-04)
  Destination 2  ->  Google Ads account "Finfluencers.Trade", AW-18322362149  (added 2026-07-14)

GA4:  account a16171064 / property 485294334
      web stream "Finfluencers.Trade", stream ID 10516700775
      URL https://finfluencers.trade
      measurement ID G-BLE3H05Q5T
      connected site tags: 0        status: data flowing

Google Ads:  account 635-115-7649 (ocid=8404411852)
```

`G-BLE3H05Q5T` **is** the measurement ID of GA4 property 485294334. The tag and the property are the two ends of one pipe, not two competing systems.

## The trap

Google Ads' conversion **data-source picker** lists these side by side as if they were alternatives:

| Shown as | Marked | What it actually is |
|---|---|---|
| Finfluencers.Bet — `G-BLE3H05Q5T` | *Saidile paigaldatud* (installed on site) | the Google tag |
| Finfluencers.Trade — `485294334` | *Selle kontoga lingitud* (linked to this account) | the GA4 property the same tag feeds |

Both must be selected. Until 2026-08-04 only the first was, which is why **no GA4 key event could reach Google Ads at all** — it was never a sync-latency problem, which is what it looked like from the symptoms.

## Corrected misconception

A note in the Google Ads morning-review task claimed *"the site only has a GA4 gtag, no native Google Ads AW- tag anywhere"*, and this was used as the root-cause explanation for a conversion action stuck at 0. **It is wrong.** `AW-18322362149` exists and has been a destination of the same Google tag since 2026-07-14.

Keep two layers distinct:

- **Page source** may legitimately reference only `G-BLE3H05Q5T`.
- **Tag configuration** carries the `AW-` destination regardless.

A statement about one is not a statement about the other.

## Conversion actions (2026-08-04)

| Action | Source | Role | Note |
|---|---|---|---|
| Registreerumine (Lehe laadimine finfluencers.trade/signup) | Website | **Primary** | the real bidding signal |
| Registreerumine (sign_up) | Website | Secondary | duplicate of the above; keep secondary |
| Lehevaatamine (finfluencers.trade/) | Website | Inactive / secondary | legacy, rule `finfluencers.trade/*`, permanently 0 |
| Lehevaatamine (GA4 event `view_page`) | GA4 | Primary *within its goal* | imported 2026-08-04, counting "one per click", 30-day window |

**Safety note on the last row:** it is marked Esmane and Google greys out the secondary toggle. That is safe *only* because the "Lehevaatamine" goal is not an account-default goal and is attached to **0/2 campaigns**, so it never feeds Smart Bidding. If that goal is ever attached to a campaign, a near-universal pageview signal starts driving bids. Re-check the 0/2 each review.

GA4 also emits `performance_chart_viewed`, not yet imported — a stronger intent signal than a pageview.

## Why the legacy page-view action reads 0

Its scope was never the problem — the rule `finfluencers.trade/*` correctly covered the whole site. The likeliest cause is that Google Ads' code-less URL-rule page-load detection does not fire reliably when `AW-` reaches the page only as a *tag destination* rather than an explicit on-page config. The GA4-event route sidesteps page-load detection entirely, which is why it is the correct replacement.

## Lessons that generalise

- When a UI shows a **name**, verify the underlying **ID** before drawing conclusions. Names go stale after rebrands; IDs do not.
- When replacing a broken tracking object, read the **original's actual scope** first (URL pattern, targeting, audience). Do not silently narrow it because a narrower version "feels" reasonable.
- Distinguish *page source* from *tag configuration* before declaring a tag absent.

## Related pages

- [[products/finfluencer-trade]]
- [[projects/finfluencer-tracker]]
- [[projects/gor-blog]]
- [[concepts/subscription-entitlement-ssot]]
