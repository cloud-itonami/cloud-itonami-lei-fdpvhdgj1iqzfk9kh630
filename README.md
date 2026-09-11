# cloud-itonami-lei-fdpvhdgj1iqzfk9kh630

> **Independent third-party archive/analysis. Not affiliated with, endorsed by, or sponsored by Eastman Chemical Company.**

This repository archives the publicly published Terms of Use / Terms and Conditions of
**Eastman Chemical Company**, with source-url and retrieval-date provenance, per
[ADR-2607110300](https://github.com/com-junkawasaki/root/blob/main/90-docs/adr/2607110300-cloud-itonami-lei-corporate-tos-catalog.md)
(`cloud-itonami-lei-corporate-tos-catalog`, `com-junkawasaki/root`). It is a read-only
reference/archive repository — it does not act, propose, or execute anything on the
company's behalf, and is not a governed Advisor/Governor actor.

## Company identity

- **Legal name**: Eastman Chemical Company
- **LEI (ISO 17442)**: [FDPVHDGJ1IQZFK9KH630](https://search.gleif.org/#/record/FDPVHDGJ1IQZFK9KH630) (GLEIF-verified)
- **Jurisdiction**: US-DE (incorporation; headquarters in Kingsport, US-TN — see below)
- **Website**: https://www.eastman.com
- **Ticker**: EMN (NYSE)

## Contents

- `80-data/public/tos.journal.edn` — EDN quad-log of archived Terms of Use documents,
  each entry carrying `:tos/full-text`, `:tos/source-url`, `:tos/retrieved-at`,
  `:tos/sha256`, `:tos/doc-type`, and a `:tos/supersedes` chain for future revisions.
- `NOTICE` — copyright/attribution statement for the archived third-party text.
- `blueprint.edn` — machine-readable company identity record.
- `facts.edn` — 24 verified registry facts with per-fact provenance. **Generated** — see below.
- `scripts/verify-facts.cljk` — re-fetches every source `facts.edn` cites and fails if
  the live record disagrees. Vendored from `com-junkawasaki/root`
  (`scripts/lei-verify-facts.cljs`); fix issues in the canonical and re-vendor.

## Verifying the record

The identity table above used to be assertions with nothing in the repository
behind them. `facts.edn` now carries them as data, and every value in it was read
out of a public registry response whose URL and retrieval time sit next to the
value:

```
kbb --backend sci scripts/verify-facts.cljk           # check the recorded facts against the live sources
kbb --backend sci scripts/verify-facts.cljk --write   # re-fetch and rewrite facts.edn
```

Eleven GLEIF/ISO URLs were fetched and 24 facts recorded — the LEI record
(legal name as GLEIF spells it, **`EASTMAN CHEMICAL COMPANY`**, upper case,
`en`; entity **ACTIVE**, registration **ISSUED**, **`FULLY_CORROBORATED`** /
`CONFORMING`; two different addresses recorded separately — the legal address
`C/O THE CORPORATION TRUST COMPANY, CORPORATION TRUST CENTER, 1209 ORANGE ST,
19801, WILMINGTON, US-DE, US`, a registered-agent address in Delaware, and the
headquarters `200 South Wilcox Drive, 37660-5280, Kingsport, US-TN, US`; entity
creation date `1993-07-29` — the Delaware incorporation ahead of the spin-off
from Eastman Kodak completed at the start of 1994, not the 1920 founding of
Tennessee Eastman the company's own history describes, which predates the
entity that holds this LEI; initial LEI registration `2012-06-06`, last updated
`2025-10-23`, next renewal `2026-10-29`; no BIC — a chemicals manufacturer, not
a bank; OpenCorporates id `us_de/2345566`, S&P Global id `109103`), its ISIN
mapping (**10** instrument identifiers, read from `meta.pagination.total` of
the cited page — the whole list fits that single page, so each one is also
mirrored as its own `:fact/kind :security` entity; all ten begin `US277432`,
the issuer's CUSIP-6 prefix), its managing LOU and LEI-issuer accreditation
(**Bloomberg Finance L.P.**, itself a US-DE entity, accredited 2017-04-13 —
the LEI's initial registration in 2012 predates this LOU's GLEIF accreditation
by nearly five years, so the record predates the accreditation regime it now
sits under; the transfer history itself is not in the record), registration
authority `RA000602` (**Division of Corporations, Department of State** —
`corp.delaware.gov`, serving Delaware only — where the entity is file number
`2345566`), ISO 20275 legal form `XTIQ` (**`Corporation`**, US-DE), and **both
consolidation levels**: no parent at either level, each level carried as a
reporting-exception entity with category
`DIRECT_ACCOUNTING_CONSOLIDATION_PARENT` /
`ULTIMATE_ACCOUNTING_CONSOLIDATION_PARENT` and reason **`NO_KNOWN_PERSON`** —
GLEIF's code for "no person consolidates this entity", i.e. Eastman is itself
the top of its accounting group. Nine of the eleven URLs answered `200`; the
`direct-parent` and `ultimate-parent` endpoints answered `404` because GLEIF
publishes the *exception* side of that pair for this entity, which the checker
treats as a fact rather than a failure.

Two things in this repository were corrected by the fetch. The identity table
above and `blueprint.edn` both said the jurisdiction was `US-TN`; the LEI
record says **`US-DE`**, and two more of its own fields corroborate that
against each other — the registration authority resolves to Delaware's
Division of Corporations, and the entity's file number `2345566` is a Delaware
file number. Kingsport, Tennessee is the *headquarters* address; the registry
carries the two as separate fields precisely because they differ. Both files
now say `US-DE`.

The children are the other finding. GLEIF records **5 direct children** — and
their distribution is `BE 3, ES 1, IN 1`: zero in the United States, for a
US-listed group whose 10-K subsidiary exhibit names far more entities than
five. That is not the shape of the group — it is the shape of *LEI
regulation*: a subsidiary appears here only if it holds an LEI and reports the
relationship, and EU derivatives and insurance-distribution rules compel that
where US rules mostly do not. Three of the five names carry the marks of
acquisitions (`SOLUTIA EUROPE` and `SOLUTIA CHEMICALS INDIA PRIVATE LIMITED`
from the 2012 Solutia purchase, `TAMINCO` from the 2014 Taminco purchase). The
5 is a measured count of what GLEIF's relationship graph holds, not a census
of subsidiaries, and the README says so precisely because the difference is
easy to misread.

The check has three exit codes, not two: `0` when every cited URL answered and
every recorded value still matches, `1` when a citation broke or a value
drifted (each difference is named, with the recorded and live values side by
side), and `3` when the check could not be performed at all — `facts.edn`
missing or empty, or GLEIF unreachable at the transport level — because a check
that could not run must not look like a check that ran and found nothing.
Before this landed, all three were shown against the live API: unmodified →
`0` (`OK all 24 recorded fact(s) still match the live sources`);
`:company/jurisdiction` values edited `US-DE` → `FR` → `1`, naming
`gleif-lei-record` and `gleif-managing-lou` and `:company/jurisdiction` as
`DRIFT`; the `gleif-direct-children-count` entity deleted → `1`, naming it as
`ADDED`; `facts.edn` absent → `3` (`INCONCLUSIVE … Refusing to report a
pass`). The file was restored byte-identical afterwards (`shasum` equal).

## Design rationale

See ADR-2607110300 in `com-junkawasaki/root` (`90-docs/adr/`) for why this repo exists,
why it is keyed by LEI rather than GTIN or ticker, and why full-text archival (with
provenance) was chosen over excerpt-only storage.
