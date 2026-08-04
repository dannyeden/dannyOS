# Reusable Clinical Module Inventory

**Status:** Proposed decomposition from registered sources

This inventory identifies repeated capabilities without declaring conflicting
clinical content resolved. `Candidate` means the structure is reusable; questions,
answers, gates, and outcome rules remain source-versioned.

## Intake and follow-up modules

| Module ID | Candidate module | Evidence | Reuse boundary | Status |
| --- | --- | --- | --- | --- |
| MOD-001 | Current medical conditions | SRC-005, SRC-007, SRC-008, SRC-010 | Attributed free text or structured conditions; protocol-specific interpretation stays outside module | Candidate |
| MOD-002 | Current medication and dosage | SRC-005, SRC-007, SRC-008, SRC-010 | Collection and provenance only; interaction rules belong to Protocol Version | Candidate |
| MOD-003 | Allergy history | SRC-005, SRC-007, SRC-008, SRC-010 | Collection plus source; product-specific allergy gates remain protocol-owned | Candidate |
| MOD-004 | Sex and pregnancy pathway | SRC-005, SRC-007, SRC-008, SRC-010, SRC-020 | Shared branching mechanics; consent and eligibility are protocol-specific versions | Candidate |
| MOD-005 | Height, weight, and BMI | SRC-007, SRC-010, SRC-011, SRC-020 | Measurement and calculation reusable; thresholds remain protocol rules | Candidate |
| MOD-006 | Gallstone and cholecystectomy history/consent | SRC-010, SRC-012, SRC-019, SRC-020 | Separate facts, consents, and outcome policy | Candidate |
| MOD-007 | Prior therapy and last-dose verification | SRC-007, SRC-010, SRC-020 | Medication, date, dose, concentration, and administered amount are distinct fields | Candidate |
| MOD-008 | Health-change follow-up screen | SRC-007, SRC-011, SRC-015 | Routes material changes to Provider review | Candidate |
| MOD-009 | Side-effect and response assessment | SRC-007, SRC-011, SRC-015, SRC-020 | Captures facts and preference; does not itself prescribe or change dose | Candidate |
| MOD-010 | Treatment continuation preference | SRC-007, SRC-011, SRC-020 | Increase, decrease, stay, or change are member preferences constrained by authorization | Candidate |
| MOD-011 | Questions for Provider | SRC-005, SRC-007, SRC-010, SRC-020 | Reusable free-text collection with longitudinal provenance | Candidate |
| MOD-012 | Program-specific informed consent | SRC-005, SRC-007, SRC-008, SRC-009, SRC-010, SRC-020 | Shared consent engine; content and applicability remain versioned per protocol | Candidate |
| MOD-013 | HRT symptom and surgical history | SRC-005, SRC-006 | Menopause/HRT-specific module family, including hysterectomy and oophorectomy distinctions | Candidate |
| MOD-014 | Blood-pressure evidence | SRC-005, SRC-006, SRC-012 | Measurement provenance reusable; HRT outcomes remain protocol-owned | Candidate |

## Visit requirements

| Requirement ID | Requirement | Evidence | Status |
| --- | --- | --- | --- |
| VIS-001 | Initial female-testosterone visit is synchronous | SRC-005 and SRC-018 | OPEN — CLINICAL because SRC-012 says offering is not active |
| VIS-002 | AutoRx requires Provider follow-up when health changes or significant side effects occur | SRC-011 and SRC-015 | Candidate confirmed workflow; exact clinical triggers require review |
| VIS-003 | AutoRx requires a new Provider reauthorization visit at day 180 | SRC-015 and SRC-021 | Candidate confirmed workflow; modality not stated |

## Lab requirement candidates

| Requirement ID | Program | Candidate rule | Evidence | Status |
| --- | --- | --- | --- | --- |
| LAB-001 | Compounded GLP-1 weight loss | No required gate; optional labs | SRC-012 | OPEN — CLINICAL for mapping to Care and conflict review |
| LAB-002 | GLP-1 microdosing | No labs required | SRC-012 and SRC-019 | Candidate |
| LAB-003 | HRT ages 35–39 | FSH, estradiol, serum HCG, prolactin, TSH, and AMH at onset | SRC-012 | OPEN — CLINICAL due CON-002-005 |
| LAB-004 | Female testosterone | Baseline and every-three-month total testosterone | SRC-005 and future-state text in SRC-012 | OPEN — CLINICAL due availability conflict |
| LAB-005 | Metabolic support | No baseline lab versus six-month GH monitoring | SRC-012 and SRC-013 | OPEN — CLINICAL due CON-002-011 |
| LAB-006 | Anti-aging Metformin with CKD report | Labs required, exact panel and freshness unstated | SRC-012 | OPEN — CLINICAL |

## Medical media and document requirements

| Requirement ID | Type | Requirement | Evidence | Status |
| --- | --- | --- | --- | --- |
| MED-001 | Document image | Government-issued photo ID, including reverse when required | SRC-005 and SRC-008 | Candidate Identity verification requirement; not clinical media |
| MED-002 | Medication-label image | Clear full-label photo when medication dose or concentration is uncertain | SRC-007 | Candidate conditional Source Artifact requirement |
| MED-003 | Dose education image | Visual explaining dose versus concentration | SRC-007 | Candidate educational media, not member evidence |

No source in SRC-004 establishes hair, dermatology, body, or other diagnostic
photo requirements. Those remain absent rather than inferred.

## AutoRx workflow decomposition

SRC-015 supports separate canonical objects for:

1. Provider-authored Titration Plan Authorization with effective and expiry time;
2. pharmacy-neutral dose category and allowed progression;
3. pharmacy-specific Formulation, Package, and SKU mapping;
4. member Check-In and attributable responses;
5. Prescription Review/Issuance constrained by the authorization;
6. Fill Request and financial Order;
7. hard-stop Guard Results for check-in, timing, category, and follow-up; and
8. reauthorization as a new Clinical Decision rather than extension in place.

The wording “API prescribing” in SRC-015 remains disputed. Automation MAY execute
an authorized transition only if Clinical and Compliance confirm that the
Provider's authorization legally and clinically covers the exact future
Prescription. Otherwise every Fill requires a Provider-owned prescription action.
