# Package 002 Gap Register

**Status:** Active

Gaps are missing, ambiguous, or conflicting source facts. They are not proposed
defaults. A gap closes only with a registered source or explicit decision from the
listed owner.

| Gap ID | Owner | Gap | Impact | Closure evidence |
| --- | --- | --- | --- | --- |
| GAP-002-003 | OPEN — CLINICAL | Care's exact lab-free scope is not documented | Cannot distinguish no required labs from optional or monitoring labs | Source location or explicit Clinical decision |
| GAP-002-004 | OPEN — CLINICAL | Optimize's required lab tests, timing, freshness, thresholds, and exception policy are unknown | Cannot create or evaluate the lab gate | Source locations or explicit Clinical decision |
| GAP-002-005 | OPEN — PHARMACY | SRC-003 provides exact row references but no pharmacy-native product or SKU identifiers | Canonical configurations cannot bind to native dispensing identities | Native identifiers or Pharmacy approval of source-row identity |
| GAP-002-006 | OPEN — CLINICAL | Synchronous-visit coverage is incomplete: female testosterone is identified, but other Product mappings and modality rules are unstated | Cannot complete evaluation workflows | Clinical source or decision for each Product |
| GAP-002-007 | OPEN — CLINICAL | ID and medication-label requirements are identified, but complete Product-specific photo, media, and document coverage is unstated | Cannot complete evidence collection | Clinical review of module inventory and product mapping |
| GAP-002-008 | OPEN — CLINICAL | Intake candidates are extracted, but authoritative Product composition and conflicting branches are unresolved | Cannot publish Questionnaire Compositions | Clinical approval after conflict resolution |
| GAP-002-009 | OPEN — COMMERCIAL | The 50 normalized medication names and Care/Optimize placement are proposed, not commercially approved | Cannot complete Product-to-Protocol or marketplace coverage | OD-086 and OD-087 decisions |
| GAP-002-010 | OPEN — COMPLIANCE | The bundle does not establish signed approvals, effective dates, or document supersession | Cannot determine which candidate rule was effective at a historical time | Approval and effective-date evidence |
| GAP-002-011 | OPEN — CLINICAL | Twenty-two material source conflicts remain assigned in the Source Conflict Register | Cannot publish affected Protocol Versions | Recorded resolution for every conflict |
| GAP-002-012 | OPEN — PHARMACY | Nineteen source groupings have incomplete, multi-valued, or conflicting formulation/package semantics | Cannot approve all proposed formulations and configurations | OD-093 and OD-095 decisions |
| GAP-002-013 | OPEN — COMPLIANCE | State and jurisdiction availability is absent from SRC-003 | No configuration is state-launch-ready | OD-089 decision and authoritative routing source |
| GAP-002-014 | OPEN — COMMERCIAL | Pharmacy pricing fields are not final retail prices and marketplace visibility is unstated | No concept is commercially launch-ready | OD-087, OD-088, and OD-092 decisions |
| GAP-002-015 | OPEN — CLINICAL | Thirty-eight canonical medication concepts have no supporting clinical documentation in SRC-004 | Intakes, contraindications, labs, visits, and protocols cannot be assigned | OD-094 decision and approved clinical sources |

## Resolved source dependencies

| Gap ID | Resolution | Evidence |
| --- | --- | --- |
| GAP-002-002 | Clinical-documentation bundle received and registered | SRC-004 and file-level SRC-005 through SRC-022 |
| GAP-002-001 | Eden Pharmacy formulary received, hashed, and normalized | SRC-003 and source-row register |

## Escalation rule

First search registered sources. If a source answers the gap, record the exact
location and obtain the authoritative review. If sources remain silent or conflict,
escalate to the listed owner and add the resulting decision to the Canon Decision
Log or Open Decision Register. Do not infer the answer from adjacent products.
