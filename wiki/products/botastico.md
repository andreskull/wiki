---
type: product
title: "botastico"
product: botastico
project: null
created: 2026-04-06
updated: 2026-04-06
tags: [botastico]
---

# botastico

> **Stub** — repos not yet indexed. To be populated during next bootstrap pass once botastico repos are accessible.

## Component repos

- `botastico`
- `botastico-api`
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
./deploy-api.sh prod-west1
```

AI agents should only assist with **preparing** the code for deployment (writing, testing, reviewing),
never with executing the deployment itself.
