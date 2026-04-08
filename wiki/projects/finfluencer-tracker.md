---
type: project
title: "finfluencer-tracker"
product: finfluencer-trade
project: finfluencer-tracker
created: 2026-04-06
updated: 2026-04-06
tags: [finfluencer, auth, landing, vercel, supabase, react]
---

# finfluencer-tracker

Part of [[products/finfluencer-trade]]. Vite/React SPA on Vercel — auth, billing, product UI, and marketing landing routes; see **Purpose and role** below.

## Product

[[products/finfluencer-trade]]

## Purpose and role

Vite + React + TypeScript SPA on **Vercel**: auth, Stripe billing, logged-in product (signals, leaderboard, instruments, onboarding), and **marketing landing** routes in the same deploy. Browser talks to **Supabase** (Auth, Postgres, Edge Functions); pipeline analytics originate in **BigQuery** ([[projects/gor_dagster]]) and are intended to reach the app via Supabase sync (see repo `docs/architecture/data-layer.md`).

## Tech stack

- **Frontend:** React 18, Vite, Tailwind, shadcn/ui, TanStack Query, React Router
- **Hosting:** Vercel (SPA + `api/` serverless for OG meta)
- **Supabase (PostgreSQL):** two separate projects — **production** and **development** — for app and auth data (isolated from each other)
- **Payments:** Stripe (Checkout / portal; webhooks via Supabase Edge Functions)
- **Pipeline / analytics data:** shared production **BigQuery** ([[entities/bigquery]]); app does not query BQ from the browser — see [[projects/gor_dagster]] `docs/architecture/supabase-sync-architecture.md`

## Architecture docs (source of truth)

In-repo: **[WIKI.md](file:///Users/andreskull/finfluencer-tracker/WIKI.md)** and **`docs/architecture/`** — [README](file:///Users/andreskull/finfluencer-tracker/docs/architecture/README.md), [system-overview](file:///Users/andreskull/finfluencer-tracker/docs/architecture/system-overview.md), [data-layer](file:///Users/andreskull/finfluencer-tracker/docs/architecture/data-layer.md). Cross-subdomain auth: [auth-sharing-landing-app.md](file:///Users/andreskull/finfluencer-tracker/docs/auth-sharing-landing-app.md).

## Wiki sync

The vault indexes **WIKI.md** and all of **`docs/`** except **`docs/features/`** (includes **`docs/architecture/`** and e.g. **`docs/auth-sharing-landing-app.md`**). Run **`wiki sync finfluencer-tracker`** after substantive doc updates.

## Related pages

- [[products/finfluencer-trade]]
- [[projects/gor_dagster]]
- [[projects/gor-blog]]
