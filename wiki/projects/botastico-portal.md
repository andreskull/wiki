---
type: project
title: "botastico-portal"
product: botastico
project: botastico-portal
created: 2026-11-12
updated: 2026-11-12
tags: [botastico, nextjs, turborepo, vercel, cloud-functions, slack, shadcn]
---

# botastico-portal

Turborepo (`turbo` + `npm@11.0.0` workspaces) monorepo for the [[products/botastico]] web applications and the Slack Events Google Cloud Function.

## Product

[[products/botastico]]

## Purpose and role

Customer-facing portal (`https://botasti.co`), internal admin console, agent playground, and Slack Events webhook processor. Next.js API routes in `apps/client` wrap [[projects/botastico-api]] with server-side credentials, Firebase auth, GCP storage/monitoring helpers, and Stripe billing integration.

## Tech stack (key items)

- **Monorepo:** Turborepo (`turbo`), `npm@11.0.0` workspaces
- **Web apps:** Next.js `13.5.4` App Router, TypeScript, Tailwind CSS, shadcn/ui (`apps/client/components/ui/`), Cypress E2E (`cypress/e2e/`)
- **Hosting:** Vercel (`apps/client/vercel.json`, including `/api/internal/warm-prompts`); `development` branch pushes build Vercel previews, `main` serves production (`https://botasti.co`)
- **Cloud Function:** `apps/functions/slack-events` — Node 20 Google Cloud Function Gen2 (`@google-cloud/functions-framework`, entry point `processSlackEvent`, CloudEvent / Pub/Sub), compiled from `src/` to `dist/` with Babel (`node deploy.js staging|production`)

## Architecture summary

- **`apps/client` (`:3000`):** Primary customer-facing Next.js App Router app (`app/`, `page-components/`, `components/`, `python-api/` wrappers calling `botastico-api`, `firebase/`, `gcp/`, `stripe/`). Local secrets live in `apps/client/.env.local`. Add shadcn components from inside `apps/client` (`npx shadcn@latest add <name>`).
- **`apps/admin` (`:3001`):** Internal admin Next.js `13.5.4` app.
- **`apps/playground` (`:3002`):** Agent playground Next.js `13.5.4` app (currently has no `.eslintrc`, so root `npm run lint` triggers an interactive `next lint` prompt; non-interactive lint runs `npm --prefix apps/client run lint && npm --prefix apps/admin run lint`).
- **`apps/functions/slack-events`:** Slack Events Cloud Function Gen2 (`jest.setup.js` currently needs an ESM `jest` import fix and `dist/` mock ignore before `npm --prefix apps/functions/slack-events test` runs cleanly).
- **`apps/web`:** Minimal legacy `app/faq` stub directory (no `package.json` — not the main web app).
- **Shared packages:** `packages/shared/ui` (`ui`) and `packages/config/eslint-config-custom`.

## Key decisions

- **2026-11-12 — Switched to `ai-factory` (`905514b` on `development`):** Replaced five guessed `.cursor/rules/*.mdc` files (which had misidentified `apps/web` as the main app) with a verified 36-line [`AGENTS.md`](file:///Users/andreskull/botastico-portal/AGENTS.md) (`Agent branch: development`, `CLAUDE.md` = `@AGENTS.md`, `.gitignore` += `docs/features/*/logs/`). Agents commit and push on `development` once `client`/`admin` lint and `npm run build` pass, never run `vercel deploy` / `vercel --prod` from the CLI, and leave `main` and production Cloud Function deploys to Andres.

## Completed features (indexed)

*(No wrapped `docs/architecture/features/` pages yet; temporary specs under `docs/features/` are indexed after `/wrapup`.)*

## Current status

Active on `development` and `main`; switched to [[projects/ai-factory]] on **2026-11-12** (`905514b`).

## Repo entry point

[`AGENTS.md`](file:///Users/andreskull/botastico-portal/AGENTS.md) (no separate `WIKI.md` yet).

## Related pages

- [[products/botastico]]
- [[projects/botastico-api]]
- [[projects/botastico]]
- [[projects/ai-factory]]
