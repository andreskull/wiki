---
type: product
title: "botastico"
product: botastico
project: null
created: 2026-04-06
updated: 2026-11-27
tags: [botastico]
---

# botastico

**Indexed projects:** [[projects/botastico-portal]] (updated 2026-11-27) — Turborepo web monorepo (`apps/client` on Vercel `https://botasti.co`, `apps/admin`, `apps/playground`, `apps/functions/slack-events`), switched to [[projects/ai-factory]] (`Agent branch: development`), **Public processing disclosure** wrapped **2026-11-27** (`c3be4d1` wrapup; promoted to `main` on `https://www.botasti.co` — homepage/upload public-KB notices, rewritten `/privacy-policy`, `/security`, `/data-processing-agreement`, `/terms-of-service` under unified `LegalPageLayout`, Estonian authority reply archived at `docs/compliance/2026-11-justdigi-reply.pdf`, and live website chat KB `qvoGbMp5esOw4fF7qMGs` updated), `gpt-6-luna` temperature suppression & strict `chat_image_attachments_enabled === true` upload gate (2026-11-19), local widget Cypress harness + standalone `/chatapps` & `/iframe` isolation + `gpt-4o-mini` fallback (`920477a`, `cab0e26` on `main`). [[projects/botastico-api]] (2026-05-18, updated 2026-11-27) — Flask API + `file_processor` on Cloud Run (`Agent branch: none` single-branch `main` unstaged-until-production workflow; `261922508509-compute@developer.gserviceaccount.com` granted `roles/storage.objectUser` on `gs://botastico-customers-data` on **2026-11-27** for cross-project `file_processor` ingestion; **GPT-5.1 retirement and `gpt-6-luna` rollout** wrapped 2026-11-19 on revision `00089-tbp`, `20f09d1`; **deprecated OpenAI models removal** wrapped 2026-11-15 on revision `00087`; `gh-pages` docs publish retired 2026-11-12); chat image attachments + Pub/Slack path. [[projects/botastico]] (2026-07-15) — GCP LB SSL certs, monitoring, `slack_chat_logs`. Other repos below remain lightly linked until separate wiki syncs (`botastico`, `botastico-bo`, and `botastico-react-chatapp` also carry repo-scoped `.agents/plugins/firebase/` MCP configs as of 2026-11-11).

## Component repos

- `botastico` — [[projects/botastico]] — monorepo shell / Firebase / **`slack_chat_logs`** / **SSL monitoring** ([`operations/ssl-certificates.md`](file:///Users/andreskull/botastico/operations/ssl-certificates.md))
- `botastico-api` — [[projects/botastico-api]] — Cloud Run Flask API + `file_processor` ([`AGENTS.md`](file:///Users/andreskull/botastico-api/AGENTS.md))
- `botastico-portal` — [[projects/botastico-portal]] — Turborepo Next.js apps (`client`, `admin`, `playground`) + `slack-events` Cloud Function ([`AGENTS.md`](file:///Users/andreskull/botastico-portal/AGENTS.md))
- `botastico-script`
- `botastico-stripe`
- `botastico-script-inserter`
- `botastico-opiq-case-study`

## Local Development

To start the Firestore simulator in the `botastico` folder, run the following command:
```bash
firebase emulators:start --import=./firestore-emulator-data --export-on-exit=./firestore-emulator-data
```

## Related pages

- [[wiki/overview]]
- [[projects/botastico]]
- [[projects/botastico-api]]
- [[projects/botastico-portal]]
- [[concepts/botastico-ssl-certificates]]

## Deployment Rules

> [!CAUTION]
> **AI agents must NEVER automatically deploy `botastico-api`** — neither to staging nor to production.
> All deployments of `botastico-api` must be performed manually by **Andres only**.
>
> This rule exists to prevent accidental disruption to live services. If a deployment is needed,
> instruct Andres to run the deployment script himself.

The deployment script is `deploy-api.sh` in the `botastico-api` repo:
```bash
# Staging
./deploy-api.sh staging-west1

# Production
./deploy-api.sh production-west1
```

AI agents should only assist with **preparing** the code for deployment (writing, testing, reviewing),
never with executing the deployment itself.
