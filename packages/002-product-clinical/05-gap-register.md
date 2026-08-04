# Package 002 Gap Register

**Status:** Active

Gaps are missing, ambiguous, or conflicting source facts. They are not proposed
defaults. A gap closes only with a registered source or explicit decision from the
listed owner.

| Gap ID | Owner | Gap | Impact | Closure evidence |
| --- | --- | --- | --- | --- |
| GAP-002-001 | OPEN — PHARMACY | Eden Pharmacy formulary not received | Cannot normalize Medication, Formulation, Strength, Package, or Pharmacy SKU | Registered SRC-003 plus Pharmacy confirmation |
| GAP-002-003 | OPEN — CLINICAL | Care's exact lab-free scope is not documented | Cannot distinguish no required labs from optional or monitoring labs | Source location or explicit Clinical decision |
| GAP-002-004 | OPEN — CLINICAL | Optimize's required lab tests, timing, freshness, thresholds, and exception policy are unknown | Cannot create or evaluate the lab gate | Source locations or explicit Clinical decision |
| GAP-002-005 | OPEN — PHARMACY | Pharmacy-native product and SKU identifiers are unknown | Cannot create fulfillment mappings | SRC-003 rows and Pharmacy review |
| GAP-002-006 | OPEN — CLINICAL | Synchronous-visit coverage is incomplete: female testosterone is identified, but other Product mappings and modality rules are unstated | Cannot complete evaluation workflows | Clinical source or decision for each Product |
| GAP-002-007 | OPEN — CLINICAL | ID and medication-label requirements are identified, but complete Product-specific photo, media, and document coverage is unstated | Cannot complete evidence collection | Clinical review of module inventory and product mapping |
| GAP-002-008 | OPEN — CLINICAL | Intake candidates are extracted, but authoritative Product composition and conflicting branches are unresolved | Cannot publish Questionnaire Compositions | Clinical approval after conflict resolution |
| GAP-002-009 | OPEN — DANIEL | Complete patient-facing Product catalog and canonical Care/Optimize naming are not registered | Cannot complete Product-to-Protocol coverage | Approved Product catalog source |
| GAP-002-010 | OPEN — COMPLIANCE | The bundle does not establish signed approvals, effective dates, or document supersession | Cannot determine which candidate rule was effective at a historical time | Approval and effective-date evidence |
| GAP-002-011 | OPEN — CLINICAL | Seventeen material source conflicts remain assigned in the Source Conflict Register | Cannot publish affected Protocol Versions | Recorded resolution for every conflict |

## Resolved source dependencies

| Gap ID | Resolution | Evidence |
| --- | --- | --- |
| GAP-002-002 | Clinical-documentation bundle received and registered | SRC-004 and file-level SRC-005 through SRC-022 |

## Escalation rule

First search registered sources. If a source answers the gap, record the exact
location and obtain the authoritative review. If sources remain silent or conflict,
escalate to the listed owner and add the resulting decision to the Canon Decision
Log or Open Decision Register. Do not infer the answer from adjacent products.
