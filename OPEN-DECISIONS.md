# Open Decision Register

**Status:** Active

**Owner:** Platform Architecture

**Last reviewed:** 2026-08-03

This register is the only authoritative list of unresolved architectural and
product decisions. Each item has exactly one accountable owner class:

- `OPEN — DANIEL` — product, brand, commercial, or executive direction
- `OPEN — CLINICAL` — clinical policy, professional authority, or care standards
- `OPEN — PHARMACY` — formulary, dispensing, fulfillment, or pharmacy operations
- `OPEN — COMPLIANCE` — legal, regulatory, privacy, security, or governance policy
- `OPEN — ENGINEERING` — technical design within accepted product and policy bounds

Closing an item requires a recorded decision, source or rationale, date, and Canon
changes. Moving ownership requires an explicit register edit; unowned open items
are prohibited.

## Platform governance and language

| ID | Owner | Decision required | Canon location |
| --- | --- | --- | --- |
| OD-001 | OPEN — COMPLIANCE | Initial legal jurisdictions and regulatory posture | 00, 15, 17 |
| OD-002 | OPEN — DANIEL | Named ratifiers and domain owners | 00, 17 |
| OD-003 | OPEN — ENGINEERING | Compatibility policy for published schemas | 00 |
| OD-004 | OPEN — DANIEL | Public platform name: HealthOS or DannyOS | 00 |
| OD-005 | OPEN — ENGINEERING | Architecture rules enforced automatically in CI | 01 |
| OD-006 | OPEN — DANIEL | Approval quorum for constitutional changes | 01 |
| OD-007 | OPEN — CLINICAL | Canonical distinction among therapy, treatment, and care program | 02 |
| OD-008 | OPEN — DANIEL | Whether Member remains the universal relationship term across markets | 02 |
| OD-009 | OPEN — ENGINEERING | Canonical identifier format | 03 |
| OD-010 | OPEN — CLINICAL | Precision and policy for estimated clinical occurrence time | 03 |
| OD-011 | OPEN — ENGINEERING | Boundary between Source Artifact and specialized medical-media storage | 03, 05 |
| OD-012 | OPEN — ENGINEERING | Which state machines are configured versus implemented in domain code | 04 |
| OD-013 | OPEN — COMPLIANCE | Retention and visibility of rejected transition attempts | 04 |
| OD-014 | OPEN — DANIEL | Whether Communications remains in Operations or becomes a top-level domain | 05 |
| OD-015 | OPEN — ENGINEERING | Ownership boundary between Medical Media and Source Artifact capabilities | 05 |
| OD-016 | OPEN — DANIEL | Ownership boundary between experiment assignment and analytics evaluation | 05 |

## Product, clinical, membership, and offers

| ID | Owner | Decision required | Canon location |
| --- | --- | --- | --- |
| OD-017 | OPEN — CLINICAL | Boundary between Product Variant and patient-specific treatment option | 06 |
| OD-018 | OPEN — DANIEL | Approval authority for non-clinical product content | 06 |
| OD-019 | OPEN — ENGINEERING | Compatibility rules for changing Product capability composition | 06 |
| OD-020 | OPEN — COMPLIANCE | Initial provider credentialing authority and source | 07 |
| OD-021 | OPEN — CLINICAL | Clinical Protocol governance quorum | 07 |
| OD-022 | OPEN — CLINICAL | Evidence freshness policy by clinical use | 07 |
| OD-023 | OPEN — CLINICAL | Boundary between decision amendment and reevaluation | 07 |
| OD-024 | OPEN — DANIEL | Member portal access during billing delinquency | 08 |
| OD-025 | OPEN — DANIEL | Membership proration policy ownership and allowed options | 08 |
| OD-026 | OPEN — DANIEL | Boundary between Benefit Grant and promotional credit | 08 |
| OD-027 | OPEN — DANIEL | Offer reservation and term-lock semantics | 09 |
| OD-028 | OPEN — DANIEL | Ownership and approval of Offer copy versions | 09 |
| OD-029 | OPEN — COMPLIANCE | Fairness constraints for commercial audience optimization | 09 |
| OD-060 | OPEN — DANIEL | Treatment access and fulfillment policy after dunning becomes cancellation or expiration | 08, 12, 13 |
| OD-061 | OPEN — CLINICAL | Exact clinical transition that counts as treatment initiation for the membership prerequisite | 07, 08, 12 |
| OD-062 | OPEN — CLINICAL | Care-team role taxonomy and escalation boundaries | 02, 07 |
| OD-063 | OPEN — CLINICAL | Provider recommendation-override scope, required rationale, and prohibited overrides | 07, 14 |

## Eligibility, marketplace, and treatment

| ID | Owner | Decision required | Canon location |
| --- | --- | --- | --- |
| OD-030 | OPEN — ENGINEERING | Runtime home for federated eligibility composition | 10 |
| OD-031 | OPEN — DANIEL | Cross-domain eligibility reason taxonomy and member-facing categories | 10 |
| OD-032 | OPEN — ENGINEERING | Eligibility latency and freshness expectations by use case | 10 |
| OD-033 | OPEN — DANIEL | Default marketplace product visibility policy | 11 |
| OD-034 | OPEN — COMPLIANCE | Marketplace ranking objectives and fairness review | 11 |
| OD-035 | OPEN — COMPLIANCE | Retention of marketplace Presentation Snapshots and interactions | 11 |
| OD-036 | OPEN — CLINICAL | Treatment Plan granularity across conditions and care programs | 12 |
| OD-037 | OPEN — CLINICAL | Collaborative Treatment Plan approval rules | 12 |
| OD-038 | OPEN — CLINICAL | Ownership of adherence assertions and adherence interventions | 12 |

## Pharmacy routing

| ID | Owner | Decision required | Canon location |
| --- | --- | --- | --- |
| OD-039 | OPEN — PHARMACY | Pharmacy candidate scoring governance | 13 |
| OD-040 | OPEN — PHARMACY | Required freshness for partner capability, inventory, and formulary signals | 13 |
| OD-041 | OPEN — PHARMACY | Member consent and notification requirements for rerouting | 13 |
| OD-042 | OPEN — PHARMACY | Source of truth for partner formulary and SKU data | 13 |
| OD-043 | OPEN — COMPLIANCE | Source of truth and validation policy for pharmacy jurisdiction constraints | 13 |

## Intelligence, security, privacy, and analytics

| ID | Owner | Decision required | Canon location |
| --- | --- | --- | --- |
| OD-044 | OPEN — COMPLIANCE | Initial intelligence use-case risk taxonomy | 14 |
| OD-045 | OPEN — COMPLIANCE | Intelligence release approval bodies by risk tier | 14 |
| OD-046 | OPEN — ENGINEERING | Reproducibility standard for third-party model releases | 14 |
| OD-047 | OPEN — COMPLIANCE | Member explanation and contestability standards for generated assistance | 14 |
| OD-048 | OPEN — COMPLIANCE | Security incident and breach-notification authority and escalation policy | 15 |
| OD-049 | OPEN — COMPLIANCE | Security and privacy control-framework mapping | 15 |
| OD-050 | OPEN — COMPLIANCE | Canonical data-classification levels | 15 |
| OD-051 | OPEN — COMPLIANCE | Retention authorities and policy hierarchy | 15 |
| OD-052 | OPEN — COMPLIANCE | Member-facing access and disclosure transparency | 15 |
| OD-053 | OPEN — ENGINEERING | Initial analytics semantic-layer technology | 16 |
| OD-054 | OPEN — DANIEL | Initial metric governance body | 16 |
| OD-055 | OPEN — COMPLIANCE | Identity-resolution policy for analytics | 16 |
| OD-056 | OPEN — COMPLIANCE | Boundaries and approvals for secondary data use | 16 |

## Repository and release mechanics

| ID | Owner | Decision required | Canon location |
| --- | --- | --- | --- |
| OD-057 | OPEN — ENGINEERING | Canon release, versioning, and compatibility strategy | 17 |
| OD-058 | OPEN — DANIEL | Production application repository name | 17 |
| OD-059 | OPEN — ENGINEERING | Mechanism for implementations to declare and consume a Canon version | 17 |

## Package 002 source conflicts

| ID | Owner | Decision required | Package location |
| --- | --- | --- | --- |
| OD-064 | OPEN — CLINICAL | Insulin-dependent Type 2 diabetes eligibility for GLP-1 treatment | CON-002-001 |
| OD-065 | OPEN — CLINICAL | GLP-1 dose-break intervals and restart actions | CON-002-002 |
| OD-066 | OPEN — CLINICAL | Weight-management initiation, continuation, and discontinuation BMI gates | CON-002-003 |
| OD-067 | OPEN — CLINICAL | Approved GLP-1 titration ladders, dose values, and discretion bounds | CON-002-004 |
| OD-068 | OPEN — CLINICAL | HRT age eligibility and lab gate | CON-002-005 |
| OD-069 | OPEN — CLINICAL | HRT family-cancer-history eligibility and consent routing | CON-002-006 |
| OD-070 | OPEN — CLINICAL | HRT gallbladder eligibility criteria | CON-002-007 |
| OD-071 | OPEN — CLINICAL | HRT drug-interaction outcome and Provider-review path | CON-002-008 |
| OD-072 | OPEN — CLINICAL | Approved HRT formulation routes and alternatives | CON-002-009 |
| OD-073 | OPEN — CLINICAL | Female-testosterone availability, labs, and synchronous-visit policy | CON-002-010 |
| OD-074 | OPEN — CLINICAL | Metabolic-support medication coverage and monitoring labs | CON-002-011 |
| OD-075 | OPEN — CLINICAL | Approved ondansetron dose and protocol | CON-002-012 |
| OD-076 | OPEN — CLINICAL | Clinical authority model for AutoRx plan authorization and later prescriptions | CON-002-013 |
| OD-077 | OPEN — CLINICAL | Resolution of current teleform disqualifiers against approved GLP-1 policy | CON-002-014 |
| OD-078 | OPEN — DANIEL | Canonical Care and Optimize Product identities and protocol mappings | CON-002-015 |
| OD-079 | OPEN — COMPLIANCE | Effective-date, approval, and supersession precedence for SRC-004 documents | CON-002-016 |
| OD-080 | OPEN — CLINICAL | Metabolic-support age-based dose restriction and correction of the contradictory source direction | CON-002-017 |
