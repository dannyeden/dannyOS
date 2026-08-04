# Package 002 Source Register

**Status:** Active

## Provenance requirements

Each source receives a stable source ID. Extracted facts record source ID, exact
location, source version or effective date, extraction date, extractor, authority,
and any conflict. A later source never silently replaces an earlier one.

## Registered sources

| Source ID | Source | Date received | Authority | Availability | Permitted use |
| --- | --- | --- | --- | --- | --- |
| SRC-001 | HealthOS Platform Canon & Architecture Project brief | 2026-08-03 | Daniel / platform direction | Available | Constitutional and architecture constraints |
| SRC-002 | Foundation Draft 0.1 review directive | 2026-08-03 | Daniel / product direction | Available | Ten explicit product and workflow constraints |
| SRC-003 | Eden Pharmacy formulary (`Copy of Telehealth_3Tier_Pricing_clean.xlsx`) | 2026-08-03 | Eden Pharmacy | Available | Current medication and compounded-preparation availability, source categories, formulations, strengths, package sizes, and pharmacy price fields |
| SRC-004 | Beluga clinical-documentation archive | 2026-08-03 | Mixed; see manifest | Available | Candidate clinical, questionnaire, visit, fulfillment, and partner-integration rules |

SRC-004 archive SHA-256:
`8c0a9e2b87632fc98cb50664205ee90f658efb0c716269bc117ad06b4efabda9`.

SRC-003 workbook SHA-256:
`f17b923ba37e1b62a9d7c19be4ff29799a05189d36e59874f78b08d8998d5d0c`.

The [file-level manifest](01a-source-manifest.md) assigns SRC-005 through SRC-022
and records authority and hashes for all 18 documents. Archive folder labels are
evidence of intended authority but are not proof of current effective status.

SRC-003 is authoritative only for the pharmacy facts explicitly supplied in the
workbook. It is not authority for clinical policy, state availability, retail
pricing, patient claims, membership-program mapping, or marketplace visibility.

## Confirmed extracted constraints

| Fact ID | Source | Location | Confirmed fact | Destination |
| --- | --- | --- | --- | --- |
| FACT-001 | SRC-002 | Constraint 1 | Membership is the universal treatment prerequisite | Membership and Treatment models |
| FACT-002 | SRC-002 | Constraint 2 | Care is lab-free | Product-to-Protocol Matrix; exact scope is OPEN — CLINICAL |
| FACT-003 | SRC-002 | Constraint 3 | Optimize is lab-gated | Product-to-Protocol Matrix; exact labs and timing are OPEN — CLINICAL |
| FACT-004 | SRC-002 | Constraint 4 | Membership supports multiple configurable billing options | Membership model |
| FACT-005 | SRC-002 | Constraint 5 | Authorized Providers may override recommendations | Clinical and Intelligence models |
| FACT-006 | SRC-002 | Constraint 6 | Care-team authority alone cannot prescribe | Identity and Clinical models |
| FACT-007 | SRC-002 | Constraint 7 | Treatment history is never overwritten | Core and Treatment models |
| FACT-008 | SRC-002 | Constraint 8 | Product, Formulation, and Pharmacy SKU are separate | Product and formulary model |
| FACT-009 | SRC-002 | Constraint 9 | Offer configuration cannot override clinical rules | Offer and Clinical models |
| FACT-010 | SRC-002 | Constraint 10 | Existing treatment continues during dunning while new Fills freeze | Membership, Treatment, and Pharmacy models |

## Source conflict policy

When sources disagree, record both facts, authority, dates, and affected objects.
Do not choose the newest or most convenient source automatically. Pharmacy owns
dispensing and formulary facts; Clinical owns care policy; Daniel or delegated
Commercial leadership resolves product and commercial direction; Compliance
resolves regulated-policy interpretation.

## Confidentiality and repository handling

The clinical bundle labels multiple files private/confidential and limits reuse
to Beluga service integration; the pharmacy workbook is also handled as a private
source. Raw files remain outside Git. This repository records only the minimum
derived architecture, provenance, hashes, and summarized conflicts needed to
design HealthOS. Any future raw-source storage requires an explicit Compliance
decision and access-control design.
