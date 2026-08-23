# Domain Reference

Standalone lookup table for the risk/control domains used across this
project — created so every service's sample data (and any automation
built against it) references the **same** domain vocabulary instead of
each service inventing its own. First consumer:
[services/it-risk-issues-management/](../../services/it-risk-issues-management/)
(as `Risk_Category`) and
[services/response-management/](../../services/response-management/) (as
`Domain`) both draw from this list.

The actual lookup data is [domains.csv](domains.csv) — `Domain,
Subdomain, Domain_Owner`. Treat it the same way as any other reference
data in this repo: synthetic Domain Owner names, safe to extend, but
confirm against the real control taxonomy and named owners before this
backs anything real.

## Shape

- **Domain** — the 8 top-level risk/control categories already
  established by IT Risk Issues Management's `Risk_Category` field:
  Access Management, Data Protection, Network Security, Third Party Risk,
  Patch Management, Application Security, Cloud Security, Governance &
  Documentation.
- **Subdomain** — 3 per Domain (24 rows total), a finer-grained bucket
  within it. Invented to give the taxonomy enough texture to be useful,
  not an authoritative breakdown.
- **Domain_Owner** — one named owner per Domain (repeated across its 3
  Subdomain rows). This is a different role than a record-level `Owner`
  (the person accountable for a specific finding/issue/inquiry) — the
  Domain Owner is the standing SME accountable for the control domain
  itself, e.g. who a Findings analyst or a Response Management analyst
  would route a domain-specific question to.

## Usage convention

Consuming datasets should tag records with the **Domain** value only
(matching the existing `Risk_Category` convention) — Subdomain and Owner
are looked up from this file when needed, not duplicated onto every
record. Keeps the domain taxonomy edited in exactly one place.

## Why a standalone file

Both IT Risk Issues Management and Response Management data need the
same 8 domains, and future services will too — a shared lookup file
means adding a 9th domain or renaming one happens once, here, rather than
being reconciled across every service's data folder by hand.
