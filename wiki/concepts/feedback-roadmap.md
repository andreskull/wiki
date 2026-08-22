---
type: concept
title: "In-app feedback & public roadmap"
product: finfluencer-trade
project: finfluencer-tracker
created: 2026-08-22
updated: 2026-08-22
tags: [feedback, roadmap, supabase, resend, rls, notify]
---

# In-app feedback & public roadmap

## Definition

Native `/feedback` board on `https://finfluencers.trade` (replaced Featurebase, **2026-06-02**). Authenticated users submit and vote; the admin (`info@finfluencers.trade`) moves posts through **Under Review → Planned → In Progress → Shipped**, or **Declined** with a public note.

**Declined with note (2026-08-22):** a request can be turned down in the open instead of deleted silently. The note is mandatory when status is `declined` (table CHECK), optional on every other status, and visible to every visitor. Declined posts stay on the board by default, sorted below active items. Existing votes are kept; new votes stop.

**Notify intent:** `notify_requested_at` is stamped only when the admin checks “Email the submitter.” The trigger watches that column; the edge function compares old vs new to decide whether to send. A status-only gate cannot express “notify with no status change” or “status changed, don’t email.”

## Relevance

Silent delete was the only reject tool, so considered-and-rejected requests either vanished or sat in Under Review forever. The public note is meant to stop duplicate submissions. Most production posts have `user_id` NULL (`ON DELETE SET NULL` after author deletion) — the UI disables notify with an explanation rather than pretending mail will go out.

## Which projects use it

- [[projects/finfluencer-tracker]] — board, admin panel, `notify-moderators` edge function (deployed **twice**, once per Supabase project)

## Standing rules

- Recreate `tr_feedback_posts_notification` only. **Never rewrite `handle_feedback_notification()`** — it holds the per-environment edge-function URL ([dev-prod divergence §B](file:///Users/andreskull/finfluencer-tracker/docs/architecture/dev-prod-supabase-divergence.md)).
- Silent save **omits** `notify_requested_at` from the UPDATE. Writing `null` still fires `UPDATE OF` because the column was mentioned.
- `NOTIFY pgrst, 'reload schema'` after a migration that adds columns, or PostgREST 404s writes that exist.
- Vote insert/delete allowlist is `under_review`/`planned` on **both** policies (`VOTABLE_STATUSES`). Delete is status-only, not category.
- Policy tests need a **real user JWT**. Service role bypasses RLS. Dev password sign-in is captcha-gated — mint sessions the way `e2e/auth.setup.ts` does.
- When `user_id` is null: disable the notify checkbox, do not hide it; `deriveNotifyChecked(..., hasSubmitter=false)` is always false. Do not change the edge skip into an error.
- HTML-escape every interpolated email field (`shared/escapeHtml.ts`, `&` first). Title and description are submitter-authored.
- Creator auto-upvote still records; the moderator upvote alert for a self-vote is suppressed.

## Related concepts / sources

- Increment: [feedback-decline-with-note.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/feedback-decline-with-note.md)
- Living system: [feedback-system.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/feedback-system.md)
- Migration: `supabase/migrations/20260819100000_feedback_decline_with_note.sql`

## Related pages

- [[projects/finfluencer-tracker]]
- [[products/finfluencer-trade]]
