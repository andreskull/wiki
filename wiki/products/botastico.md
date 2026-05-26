---
type: product
title: "botastico"
product: botastico
project: null
created: 2026-04-06
updated: 2026-05-18
tags: [botastico]
---

# botastico

**Indexed project:** [[projects/botastico-api]] (2026-05-18) — Flask API on Cloud Run; chat image attachments + Pub/Slack path documented. Other repos below remain lightly linked until separate wiki syncs.

## Component repos

- `botastico` — monorepo shell / Firebase / **`slack_chat_logs`** Cloud Function ([`slack_chat_logs/`](file:///Users/andreskull/botastico/slack_chat_logs))
- `botastico-api` — [[projects/botastico-api]]
- `botastico-script`
- `botastico-portal`
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
- [[projects/botastico-api]]

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
