---
name: venturemind-watch
description: VentureMind recurring watch — monitor regulatory changes, treaty updates, and compliance shifts across target jurisdictions.
metadata:
  source: https://github.com/Houdinnie/venturemind
  attribution: Houdinnie / VentureMind
  license: MIT
  usage: vendored
---

# VentureMind Regulatory Watch

> Monitor and alert on regulatory changes, treaty updates, and compliance shifts across VentureMind's target jurisdictions.

## When to Use

Use this as a recurring task — daily or weekly — to keep the jurisdiction knowledge base current.

## Watch Targets

| Domain | Sources | Frequency |
|--------|---------|-----------|
| Tax treaties | OECD, UN treaty databases, national tax portals | Weekly |
| Entity formation law | State.gov portals, Companies House, DED Dubai | Weekly |
| KYC/AML rules | FATF, FinCEN, local regulator updates | Daily |
| Residency programs | Government immigration portals, official gazettes | Monthly |
| Crypto regulation | SEC, ESMA, FCA, MAS regulatory updates | Daily |
| Banking rules | Central bank circulars, OFAC sanctions list | Daily |

## Alert Protocol

If a change is detected:
1. Flag the change with severity (LOW / MEDIUM / HIGH / CRITICAL)
2. Assess impact on existing founder profiles
3. If HIGH or CRITICAL → alert Watchdog Agent + trigger Compliance Auditor review
4. Log to `notes/regulatory-watch/<YYYY-MM>/`

## Output

- `notes/regulatory-watch/<YYYY-MM>/alerts.md` — weekly digest
- `notes/regulatory-watch/<YYYY-MM>/critical-alerts.md` — immediate alerts
- `outputs/<slug>/regulatory-update.md` — founder-facing update when needed