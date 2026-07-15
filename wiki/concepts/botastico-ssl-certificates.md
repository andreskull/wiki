---
type: concept
title: "Botastico SSL certificates"
product: botastico
project: botastico
created: 2026-07-15
updated: 2026-07-15
tags: [botastico, ssl, gcp, load-balancer, monitoring]
---

# Botastico SSL certificates

## Definition

TLS for Botastico customer-facing widget delivery splits across three tiers:

1. **GCP load balancer managed certs** (`chatapps`, `assets`, staging/test assets) — ~90-day Google-managed certs on project `botastico` load balancers. Auto-renew ~30 days before expiry when every SAN passes DNS validation.
2. **Google-hosted API** (`api.botasti.co`, `staging.api.botasti.co`) — provider-managed, auto-renew.
3. **Vercel** (`www`, apex, staging) — provider-managed, auto-renew.

## Relevance

July 2026: expired LB certs caused `ERR_CERT_DATE_INVALID` on `chatapps.botasti.co/main.js` — widget missing on **all** embedded customer sites (e.g. rattaproff.ee). Custom launcher icons on `assets.botasti.co` failed similarly.

Root cause was **failed auto-renew**, not the 90-day limit: orphan domain `development.assets.botasti.co` on the old assets cert blocked DNS validation (`FAILED_NOT_VISIBLE`).

## Which projects use it

- [[projects/botastico]] — certs, monitoring, runbook
- `botastico-script` — builds `main.js` served from `chatapps`
- `botastico-script-inserter` — WordPress plugin injecting widget script
- `botastico-portal` — uploads icons to `assets`

## Operator model

**Trust GCP auto-renew for LB certs.** Alert only when:

- Uptime check fails (minutes)
- GCP `managed.status` ≠ `ACTIVE` or any `domainStatus` ≠ `ACTIVE`
- LB host ≤7 days from expiry without renewal (`GCP_RENEWAL_LATE_DAYS`)
- Third-party host ≤14 days (backup; rarely fires)

Do **not** alert on routine "89 days left" for GCP LB hosts.

## Invariant

Never add a domain to a GCP managed cert unless its DNS A record matches the target load balancer forwarding-rule IP.

## Sources

- [ssl-certificates.md](file:///Users/andreskull/botastico/operations/ssl-certificates.md) — runbook
- [[decisions/botastico-gcp-managed-ssl-2026-07]] — stay on managed certs

## Related pages

- [[projects/botastico]]
- [[products/botastico]]
- [[projects/botastico-api]]
