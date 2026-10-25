---
type: concept
title: "Foreign Listing Resolution"
created: 2026-10-25
updated: 2026-10-25
product: finfluencer.trade
project: gor_dagster
status: active
tags:
  - architecture
  - resolution
  - instruments
  - canada
  - eodhd
  - openfigi
---

# Foreign Listing Resolution

Natively resolves spoken non-US company names and ticker mentions onto their rightful foreign exchange listings (starting with Canada: TSX and TSXV), instead of mis-resolving them onto US OTC mirror lines, ticker collisions on US exchanges, or irrelevant domestic names.

Permanent architecture record: `gor_dagster/docs/architecture/features/foreign-listing-resolution.md`. Shipped and verified in production on **2026-10-25**.

---

## The Problem

When guests on shows like BNN Bloomberg's *Market Call* talked about Canadian equities, the resolver previously exhibited several systematic errors:
1. **US OTC redirects:** Spoken Canadian companies without a direct US listing were mapped to FIGI-less US OTC ticker rows created on the fly (e.g. `PRLPF`, `WSPOF`, `CNSWF`), which cannot be priced accurately via the US bulk feed.
2. **Cross-border ticker collisions:** Tickers like `SAP` (Canadian dairy giant Saputo Inc on TSX) collided with unrelated foreign companies trading in the US (SAP SE on NYSE).
3. **Contained brand-name traps:** Spoken single-token brand names like `Magna` matched unrelated candidates with longer names like `MAGNA MINING INC` (`NICU`) on token containment boosts.
4. **Foreign price staleness:** Foreign exchange rows cannot ride the US bulk feed (keyed by bare ticker) and ended up frozen without daily price updates.

---

## Architectural Solution

### 1. Listing Profile Registry & Symbol Cache
`gor_dagster/registries/listing_profiles.py` defines listing profiles (`TO` for Toronto Stock Exchange, `V` for TSX Venture Exchange).
`ExchangeSymbolIndex` loads and caches active EODHD exchange ticker lists (`US`, `TO`, `V`) on disk and in memory. Canonical exchange codes are stored as `exchange_code = 'TO'` or `'V'`. EODHD ticker symbols map `-` instead of `.` (e.g. `RCI-B.TO`, `U-UN.TO`).

### 2. Multi-Stage Resolution Logic
`gor_dagster/assets/instrument_resolution.py` incorporates listing profiles:
- **Exact Ticker with Listing Profile:** Tickers with explicit suffixes or canonical exchange matches resolve directly against TSX/TSXV listings.
- **Listing Evidence:** Spoken `.UN` or `.TO` cues qualify listing candidates directly.
- **EODHD Name Search with Profile Redirects:** Discovered names match TSX/TSXV profiles with composite FIGI verification.

### 3. Listing Policy (`choose_listing`)
`gor_dagster/services/listing_policy.py` governs share-class selection:
- **Single-listed Canadian equities:** Kept on their profile (`.TO` / `.V`).
- **Dual-listed Canadian equities with liquid US primary lines:** Cleanly redirected to their primary US ticker (`.US`).
- **OTC lines and Canadian Depositary Receipts (CDRs):** Dropped/bypassed in favor of the actual primary listing.

### 4. Hardened Contained-Name Rule
`gor_dagster/utils/contained_company_name.py` prevents auto-accepting single-token brand names against longer candidate names (`Magna` vs `Magna Mining`) unless the candidate's extra tokens are standard corporate or fund descriptors (`pipeline`, `properties`, `group`, `plc`, `resources`, `partners`).

### 5. Historical Remap & Cleanup
- Repointed **465** historical prediction rows to their true TSX/TSXV listings.
- Repointed **185** aliases (Layer 0.5 replay passed).
- Purged **104** vacated FIGI-less US OTC placeholder rows from `FinancialInstrument` in BigQuery (with full JSON restore records logged).
- Daily pricing backfill and refresh implemented via `market_sync.refresh_foreign_listings` per symbol.

---

## Key Invariants

- **Never write `CN` or `CT`:** Canonical exchange codes are strictly `'TO'` or `'V'`. EODHD interprets `.CN` as the Canadian National Stock Exchange (CSE), not OpenFIGI Canada composite.
- **No US Bulk Feed:** Foreign lines never ride the bare-ticker US bulk feed; they take session bars from EODHD per symbol.
- **Anti-Collision Guard:** Spoken names that collide with distinct foreign entities sharing a ticker (e.g. Saputo vs SAP SE) are safely segregated.
