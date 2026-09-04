---
type: decision
title: "Exclude employees of registered financial entities from finfluencers.trade outreach"
product: finfluencer-trade
project: gor_dagster
created: 2026-08-31
updated: 2026-08-31
tags: [outreach, gtm, compliance, linkedin, icp, decision]
---

# Exclude employees of registered financial entities from finfluencers.trade outreach

## Context

After MoneyShow SF (25–28 Aug 2026), a Nasdaq speaker was approached with a short note about
finfluencers.trade and asked for informal feedback. He declined on compliance grounds: Nasdaq as
a self-regulatory organisation "would likely frown on any effort along those lines."

Read literally the objection is wrong on jurisdiction — an SRO's remit covers exchange members,
listed companies, and trading rules, not independent podcasters or public-data analytics. What
matters is that his personal risk tolerance for anything touching individual stock calls is zero,
and that this is structural rather than personal.

The finding generalises well beyond one contact:

- Of the twelve MoneyShow speakers who warranted personalised performance sheets, **six sat inside
  registered firms** (Evercore ISI, Freedom Capital Markets, YieldMax ETFs, Bitwise, Banríon
  Capital Management, DeCarley Trading).
- The live LinkedIn outreach cohort at the time of the decision was **14 contacts, and included
  Kevin Davitt of Nasdaq** — the same employer as the person who had just declined.
- An unsolicited third-party scorecard of a registered representative's *own* public calls is
  worse than a cold pitch: they can neither acknowledge nor correct it, and forwarding it would
  read as an endorsement they are not permitted to give.

The expensive second-order effect is measurement, not etiquette. Non-replies from this segment
are indistinguishable from disinterest and quietly bias any read on demand.

A separate, genuinely product-adjacent risk surfaced by the same conversation is **accuracy of
attributed performance claims** about named individuals. That is a defamation and claims-accuracy
surface, not an SRO one, and is tracked independently of this decision.

## Options considered

| Option | Pros | Cons |
|---|---|---|
| **Do nothing; keep sending to everyone** | No work | Keeps creating compliance problems for recipients; keeps corrupting response-rate data |
| **Soften the message for regulated contacts** ("media transparency analytics") | Feels like it preserves reach | The softened message still lands in the same compliance inbox; produces a worse pitch that also fails |
| **Classify by employer and hold regulated contacts from automated drafting** (chosen) | Removes the structural dead weight; keeps operator judgment intact; makes silence interpretable | Requires a manual classification per contact |
| **Reposition the product away from stock-pick scoring** | Would open the institutional door | Retail is the buyer; accountability framing is what works there. Wrong threat to respond to |

## Decision made

**Qualify outreach contacts by employer type, and hold anyone at a registered entity from
automated performance messaging.** Retail investors, independent podcasters, newsletter authors,
owner-operated research shops, educators, journalists, and academics remain the target.

Enforced in code rather than by convention. The Notion database *LinkedIn Finfluencer Outreach*
carries an `Affiliation` select property (`independent` / `regulated`); anything else, including
unset, holds. `scripts/export_linkedin_outreach_snapshot.py` carries it into the snapshot and
`scripts/draft_linkedin_outreach_intros.py` routes non-`independent` contacts to `manual_review`
with `fallback_reason=regulated_affiliation_hold` — no message drafted, no chart rendered.

The gate **fails closed**: unclassified contacts are held. Classifying costs one glance at a
current employer; guessing wrong costs the recipient a compliance problem.

A hold withholds the automated message, not the operator's judgment — a human may still write
something by hand.

## Consequences

- First run after the change held all 14 live contacts (none classified yet); the cohort needs a
  one-time `Affiliation` backfill in Notion.
- Response-rate measurement excludes held contacts, so traction reads become meaningful.
- Conference collateral follows the same buckets: personalised performance sheets for independents,
  generic one-pager or nothing for regulated attendees.
- Expect roughly half of any finance-conference speaker roster to be unreachable this way. Budget
  printing and intercept time accordingly.

## Affected projects and repos

| Repo | Change |
|---|---|
| `gor_dagster` | `Affiliation` gate in the outreach scripts; runbook `linkedin-outreach-intro-agent-runbook.md`; new `docs/operations/conference-outreach-playbook.md` |
| `gor-blog` | `growth_plan.md` — *Who cannot be an early design partner*; new risk-mitigation entry on misreading structural silence |
| `finfluencer-tracker` | None. `Terms.tsx` §5 already disclaims registered advisor / broker-dealer status, consistent with this decision |

## Related pages

- [[projects/gor_dagster]]
- [[projects/gor-blog]]
- [[products/finfluencer-trade]]
- [[concepts/linkedin-outreach]]
