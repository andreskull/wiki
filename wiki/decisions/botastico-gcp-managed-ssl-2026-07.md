---
type: decision
title: "Stay on GCP managed SSL certs for Botastico load balancers"
product: botastico
project: botastico
created: 2026-07-15
updated: 2026-07-15
tags: [botastico, ssl, gcp, decision]
---

# Stay on GCP managed SSL certs for Botastico load balancers

## Context

July 2026 outage: `chatapps-certificate` and `assets-certificate` expired without successful auto-renew (`PROVISIONING_FAILED_PERMANENTLY`). Customer widgets broke site-wide.

GCP managed certs have a **maximum ~90-day validity** (industry CA policy). Renewal is automatic when all domains on the cert validate.

## Options considered

| Option | Pros | Cons |
|---|---|---|
| **GCP managed certs (status quo, clean SAN list)** | Auto-renew; no manual cert uploads | 90-day cycle; one bad SAN blocks whole cert |
| **Self-managed 1-year commercial cert** | Longer validity | Manual renewal; operational burden |
| **Move widget to Cloud CDN / different host** | Different TLS path | Large migration; out of scope for incident fix |

## Decision made

**Stay on GCP managed certs** for `chatapps` and `assets` load balancers. Replace certs only when renewal fails (remediation playbook). Monitor for **renewal failure**, not routine expiry countdown.

**Date:** 2026-07-15

## Consequences

- Created `chatapps-certificate-v2` and `assets-certificate-v2` with only DNS-valid domains
- Deleted old failed certs
- Deployed `ssl_cert_monitor` (daily) + GCP Monitoring uptime checks (5 min)
- **Forbidden:** adding `development.assets.botasti.co` to any managed cert until DNS is fixed
- Next expected operator action: none until ~September 2026 auto-renew window (alerts if it fails)

## Affected projects/repos

- [[projects/botastico]]
- `botastico-script`, `botastico-script-inserter`, `botastico-portal`

## Related pages

- [[concepts/botastico-ssl-certificates]]
- [[projects/botastico]]
