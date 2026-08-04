# Package 002 Source Conflict Register

**Status:** Active

No rule listed here may become foundational until the assigned owner resolves it.
The register summarizes conflicts; reviewers must use the hashed source documents
for the exact language and context.

| Conflict ID | Decision | Owner | Topic | Conflicting evidence | Architectural effect |
| --- | --- | --- | --- | --- | --- |
| CON-002-001 | OD-064 | OPEN — CLINICAL | Insulin-dependent Type 2 diabetes and GLP-1 | SRC-012 says treatable with caution and provider discretion; SRC-019 and current teleform SRC-020 exclude it | Clinical eligibility result cannot be canonical |
| CON-002-002 | OD-065 | OPEN — CLINICAL | GLP-1 dose-break windows | SRC-012 uses less than 1 week, 1–4 weeks, and over 1 month; SRC-019 and SRC-007 use different interval boundaries and an individualized-protocol exception | Restart state machine and dose recommendation remain open |
| CON-002-003 | OD-066 | OPEN — CLINICAL | Weight-management BMI gates | SRC-012 requires BMI 25 for treatment-naive patients, includes maintenance and discontinuation thresholds that conflict internally; SRC-020 soft-disqualifies below 23; SRC-011 hard-stops at 20 or below | Eligibility and treatment-discontinuation rules remain open |
| CON-002-004 | OD-067 | OPEN — CLINICAL | GLP-1 titration ladders | SRC-012 and SRC-019 differ on doses, grouping, duration, and provider-discretion language | No titration Protocol Version may be published |
| CON-002-005 | OD-068 | OPEN — CLINICAL | HRT age and lab gate | SRC-012 limits treatment to age 35+ and requires a defined panel for ages 35–39; SRC-005 says under 40 with different service-language implications; SRC-018 says under 40 without the same eligibility rule | Lab Requirement Set and HRT eligibility remain open |
| CON-002-006 | OD-069 | OPEN — CLINICAL | HRT family-cancer routing | SRC-012 allows conditional consent for a defined first-degree-family-history case; SRC-018 routes strong family history away from HRT | HRT contraindication and consent branch remain open |
| CON-002-007 | OD-070 | OPEN — CLINICAL | HRT gallbladder criteria | SRC-012 distinguishes ongoing issues with gallbladder intact; SRC-018 uses broader current-or-recent gallbladder language | HRT eligibility remains open |
| CON-002-008 | OD-071 | OPEN — CLINICAL | HRT drug-interaction outcome | SRC-012 exits the telehealth HRT flow for listed medicines; SRC-018 says document and discuss | Guard result and provider-review authority remain open |
| CON-002-009 | OD-072 | OPEN — CLINICAL | HRT formulation routes | SRC-012 currently defaults to patch or cream and marks testosterone coming soon; SRC-005 and SRC-018 include gel, spray, oral alternatives, or active testosterone logic | Product/Formulation mapping cannot be confirmed |
| CON-002-010 | OD-073 | OPEN — CLINICAL | Female testosterone availability | SRC-012 says unavailable pending lab support; SRC-005 and SRC-018 include treatment logic, baseline/three-month labs, and an initial synchronous visit | Product availability, lab gate, and visit requirement remain open |
| CON-002-011 | OD-074 | OPEN — CLINICAL | Metabolic-support monitoring | SRC-012 says labs not required; SRC-013 requires GH testing every six months after treatment and includes tesamorelin absent from SRC-012 | Monitoring and Product coverage remain open |
| CON-002-012 | OD-075 | OPEN — CLINICAL | Ondansetron dosing | SRC-012 supplies a dose range; SRC-014 explicitly says dosing was not provided and needs clarification | No confirmed Formulation or prescribing instruction |
| CON-002-013 | OD-076 | OPEN — CLINICAL | AutoRx prescribing authority | SRC-015 calls authorization the doctor's job but calls partner API release “prescribing”; Canon requires Provider authority and forbids Care Team prescribing | Model must separate Plan Authorization, Prescription Review/Issuance, and operational release without delegating clinical authority |
| CON-002-014 | OD-077 | OPEN — CLINICAL | Teleform implementation versus approved GLP-1 policy | SRC-020 implements BMI and condition disqualifiers that differ from SRC-012, including insulin-dependent diabetes, CAD, and fatty-liver behavior | Existing code cannot be treated as protocol source of truth |
| CON-002-015 | OD-078 | OPEN — COMMERCIAL | Care and Optimize mapping | Neither name appears as a canonical patient-facing Product in SRC-004 | Sponsor constraints are confirmed, but their protocol and catalog identities remain unmapped |
| CON-002-016 | OD-079 | OPEN — COMPLIANCE | Effective status and version precedence | Folder labels and archive timestamps do not establish approval signatures, effective dates, or supersession among documents | Sources remain candidates, not ratified Protocol Versions |
| CON-002-017 | OD-080 | OPEN — CLINICAL | Metabolic-support age-based dose restriction | SRC-008 says the 0.5–0.6 dose is prohibited for patients over 40; SRC-012 and SRC-013 place the restriction on patients under 40 | The questionnaire cannot be implemented or corrected without an attributable Clinical decision |
| CON-002-018 | OD-081 | OPEN — PHARMACY | Pricing-tier meaning | SRC-003 `Formulary Pricing` defines Tier 1/2/3 as 10%, 20%, and 30% below base; `Telehealth Pricing` defines the same labels as 30%, 40%, and 50% below base | Both namespaces remain separate; no downstream pricing rule may select one by tier label alone |
| CON-002-019 | OD-082 | OPEN — PHARMACY | Bi-mix and Tri-mix duplicate identity | SRC-003 repeats each preparation under two categories with matching packages and prices but different spelling, strength wording, and total-mg values | Probable pairs remain separate configurations pending Pharmacy resolution |
| CON-002-020 | OD-083 | OPEN — CLINICAL | Oral semaglutide formulation | SRC-012 and SRC-019 document oral semaglutide without identifying suspension versus troche; SRC-003 contains both dosage forms | Neither pharmacy formulation can be assigned to the oral protocol silently |
| CON-002-021 | OD-084 | OPEN — CLINICAL | HRT composition and route coverage | SRC-003 offers variable hormone creams and troches across multiple ingredients; active and future-state HRT routes and ingredients differ across SRC-005, SRC-012, and SRC-018 | Exact Product-to-Protocol mappings remain `REVIEW_REQUIRED` |
| CON-002-022 | OD-085 | OPEN — PHARMACY | Clinical products without current Eden Pharmacy configuration | SRC-012 through SRC-014 document ondansetron, metformin, and tesamorelin, but none appears in authoritative current-availability source SRC-003 | Fulfillment source, external-pharmacy routing, or unavailability must be explicit |

## Non-conflicting confirmed architecture

- AutoRx distinguishes physician authorization from pharmacy-specific medication
  mapping and financial Order fulfillment.
- Pharmacy may change between Fills while the authorized medication category and
  dose constraints remain stable.
- Day-90 check-in and day-180 reauthorization are explicit workflow gates.
- Side effects or significant health changes route to Provider review.
- Missing medication-label detail can trigger a conditional document or photo
  verification step.
- Existing implementation behavior is evidence, not clinical authority.
- Pharmacy category labels and pricing tiers are source facts, not membership-
  program mappings or final retail-price authority.
