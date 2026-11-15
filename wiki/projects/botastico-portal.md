---
type: project
title: "botastico-portal"
product: botastico
project: botastico-portal
created: 2026-11-12
updated: 2026-11-15
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

- **2026-11-15 — Local widget E2E harness, standalone `/chatapps` + `/iframe` widget isolation, and `gpt-4o-mini` fallback (`920477a`, `cab0e26`, promoted to `main`):** After tearing down `staging.chatapps.botasti.co` LB IP, local Cypress E2E now builds `botastico-script` (`ENVIRONMENT=local`) into gitignored `apps/client/public/chat-widget/main.js` via `npm run build:widget:local` (`SCRIPT_URL=http://localhost:3000/chat-widget/main.js`), while Vercel preview (`https://staging.botasti.co`) serves the widget from `https://storage.googleapis.com/botastico-chatbot-dev/main.js` and production stays on `https://chatapps.botasti.co/main.js`. `apps/client/app/layout.jsx` suppresses the global marketing chatbot script on `/chatapps/*` and `/iframe/*` and `ChatAppWrapper.jsx` skips Next.js `<Script>` reload when `window.mountBotasticoChatbot` is already mounted. `apps/client/components/app/agents/widgets/Chat.jsx` now defaults to `gpt-4o-mini` instead of `gpt-3.5-turbo` (see [deprecated-openai-models-removal.md](file:///Users/andreskull/botastico-api/docs/architecture/features/deprecated-openai-models-removal.md); unmounted `Prices.jsx` / `FAQ.jsx` copy intentionally left untouched). Verified 24/24 live Blurby + Opiq public `/chatapps/<id>` URLs on `https://www.botasti.co`.
- **2026-11-12 — Switched to `ai-factory` (`905514b` on `development`):** Replaced five guessed `.cursor/rules/*.mdc` files (which had misidentified `apps/web` as the main app) with a verified 36-line [`AGENTS.md`](file:///Users/andreskull/botastico-portal/AGENTS.md) (`Agent branch: development`, `CLAUDE.md` = `@AGENTS.md`, `.gitignore` += `docs/features/*/logs/`). Agents commit and push on `development` once `client`/`admin` lint and `npm run build` pass, never run `vercel deploy` / `vercel --prod` from the CLI, and leave `main` and production Cloud Function deploys to Andres.

## Completed features (indexed)

| Completed | Feature | Doc |
|-----------|--------|-----|
| 2026-11-15 | Deprecated OpenAI models removal (portal `Chat.jsx` fallback + standalone `/chatapps` & `/iframe` embed isolation + local Cypress widget harness) | [deprecated-openai-models-removal.md](file:///Users/andreskull/botastico-api/docs/architecture/features/deprecated-openai-models-removal.md) |

## Current status

Active on `development` and `main`; `920477a` (`Chat.jsx` `gpt-4o-mini` fallback) and `cab0e26` (local widget E2E + standalone `/chatapps` & `/iframe` isolation) promoted to `main` and verified live on `https://www.botasti.co` (**2026-11-15**, 24/24 Blurby + Opiq public URLs); switched to [[projects/ai-factory]] on **2026-11-12** (`905514b`).

## Repo entry point

[`AGENTS.md`](file:///Users/andreskull/botastico-portal/AGENTS.md) (no separate `WIKI.md` yet).

## Related pages

- [[products/botastico]]
- [[projects/botastico-api]]
- [[projects/botastico]]
- [[projects/ai-factory]]
