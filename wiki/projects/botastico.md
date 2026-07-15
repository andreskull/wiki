---
type: project
title: "botastico (monorepo)"
product: botastico
project: botastico
created: 2026-07-15
updated: 2026-07-15
tags: [botastico, gcp, ssl, load-balancer, cloud-functions, firebase]
---

# botastico (monorepo)

Infrastructure shell and shared Cloud Functions for [[products/botastico]]. Hosts GCP load balancer SSL certs, monitoring tooling, Firebase emulator config, and **`slack_chat_logs`** (Pub/Sub → Slack audit).

## Product

[[products/botastico]]

## Purpose and role

- **Widget delivery TLS:** `chatapps.botasti.co` (widget JS) and `assets.botasti.co` (launcher icons) sit on **GCP Compute load balancers** in project `botastico` — not Vercel/Cloud Run.
- **SSL monitoring:** daily renewal-failure alerts + real-time uptime checks after July 2026 cert outage.
- **Slack audit:** `slack_chat_logs` Cloud Function (pairs with [[projects/botastico-api]] Pub/Sub `interaction-log`).

## Tech stack (high level)

- **GCP project `botastico`:** global HTTPS load balancers (`lb-production`, `lb-production-chatapps`, `lb-staging`, `lb-test`), Google-managed SSL certs
- **GCP project `botastico-eu-west3`:** Cloud Function Gen2 `ssl-cert-monitor-production` + Cloud Scheduler (daily 07:00 Europe/Tallinn)
- **Firebase:** local Firestore emulator (`firestore-emulator-data/`)

## Architecture summary

### Load balancer SSL (critical path)

| Domain | LB | Cert resource | Provider |
|---|---|---|---|
| `chatapps.botasti.co` | `lb-production-chatapps` | `chatapps-certificate-v2` | GCP managed (~90d, auto-renew) |
| `assets.botasti.co` | `lb-production` | `assets-certificate-v2` | GCP managed |
| `staging.assets.botasti.co` | `lb-staging` | `assets-certificate-v2` | GCP managed |
| `test.assets.botasti.co` | `lb-test` | `assets-certificate-v2` | GCP managed |

Other Botastico domains (`api.botasti.co`, `www.botasti.co`, …) use Google-hosted or Vercel TLS — separate from this repo's LB certs.

### SSL monitoring (July 2026)

Three layers — operator hears only when something breaks:

1. **Uptime checks** (project `botastico`, 5 min): `chatapps.botasti.co/main.js`, `assets.botasti.co/` → email on SSL/uptime failure
2. **Daily monitor** (function in `botastico-eu-west3`, checks certs in `botastico`): GCP API non-`ACTIVE`, bad domain, renewal late (≤7 days on LB hosts); third-party hosts ≤14 days backup
3. **Inventory script:** `./ssl_cert_monitor/list_certificates.sh`

Permanent runbook: [ssl-certificates.md](file:///Users/andreskull/botastico/operations/ssl-certificates.md). Wiki concept: [[concepts/botastico-ssl-certificates]].

## Key decisions

- **2026-07-15** — Stay on **GCP managed certs** for LB domains; do not switch to 1-year self-managed. Auto-renew is reliable when cert SAN lists exclude orphan domains (see [[decisions/botastico-gcp-managed-ssl-2026-07]]).
- **2026-07-15** — Never add `development.assets.botasti.co` to a managed cert until DNS points at a serving LB (caused `PROVISIONING_FAILED` on old `assets-certificate`).

## Completed work (indexed)

| Date | Work | Doc |
|---|---|---|
| 2026-07-15 | SSL cert incident fix + monitoring | [ssl-certificates.md](file:///Users/andreskull/botastico/operations/ssl-certificates.md) |

## Current status

**Healthy (2026-07-15).** `-v2` certs ACTIVE; uptime checks and alert policy deployed. Redeploy daily monitor after `main.py` renewal-only alert logic: `./ssl_cert_monitor/deploy.sh`.

## Repo entry point

[`WIKI.md`](file:///Users/andreskull/botastico/WIKI.md)

## Related pages

- [[products/botastico]]
- [[projects/botastico-api]]
- [[concepts/botastico-ssl-certificates]]
- [[decisions/botastico-gcp-managed-ssl-2026-07]]
