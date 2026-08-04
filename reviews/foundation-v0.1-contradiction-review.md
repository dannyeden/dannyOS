# Foundation Draft 0.1 — Architectural Contradiction Review

**Baseline reviewed:** `cae2459` / `canon-v0.1`

**Review date:** 2026-08-03

**Scope:** Semantic and architectural correctness only

## Method

Each sponsor constraint was compared against the preserved baseline. `PASS`
means the architecture explicitly supports the constraint. `PARTIAL` means it is
compatible but incompletely modeled. `GAP` means the baseline did not encode the
constraint. `CONFLICT` would mean the baseline encoded an incompatible rule.

## Results

| # | Constraint | Result | Baseline finding | Required action |
| --- | --- | --- | --- | --- |
| 1 | Membership is the universal treatment prerequisite | GAP | Membership was separated correctly from clinical eligibility, but no rule required membership before treatment initiation. | Add the prerequisite while preserving membership as necessary, not sufficient, for treatment. |
| 2 | Care is lab-free | GAP | The baseline intentionally contained no named-product clinical mappings. | Register as a confirmed product rule in Package 002 with source provenance; do not create a generic lab exemption. |
| 3 | Optimize is lab-gated | GAP | Lab capabilities existed, but no named-product gate was modeled. | Register the exact lab requirement and protocol mapping from clinical sources in Package 002. |
| 4 | Multiple configurable membership billing options | PARTIAL | Billing Arrangement, cadence, and versioned configuration existed, but selectable Billing Options were not explicit. | Add Billing Option as a versioned object and preserve the selected option on the arrangement. |
| 5 | Providers may override recommendations | PASS | Suggestions were explicitly non-authoritative and overrides were attributable, bounded, and reviewable. | Clarify Provider authority and distinguish recommendation override from protocol override. |
| 6 | Care-team actors cannot prescribe | GAP | The baseline prevented support roles from impersonating clinical authority but did not state the prohibition directly. | Add an explicit non-prescribing Care Team rule and require verified Provider authority. |
| 7 | Treatment history is never overwritten | PASS | Append-preserving history, immutable versions, supersession, and correction lineage were explicit. | Preserve as an invariant and add future conformance tests. |
| 8 | Product, formulation, and pharmacy SKU are separate | PARTIAL | Product and pharmacy SKU were separated; formulation was mentioned but not yet a canonical object hierarchy. | Normalize Medication, Formulation, Strength, Package, and Pharmacy SKU in Package 002. |
| 9 | Offer configuration cannot override clinical rules | PASS | This prohibition appeared in the constitutional principles, Product, Offer, Clinical, and Eligibility boundaries. | Preserve in Package 002 mapping and future configuration validation. |
| 10 | During dunning, existing treatment continues and new fills freeze | GAP | Billing and membership states were separated, and treatment history survived membership changes, but dunning and fill-freeze behavior were absent. | Add a separate billing lifecycle and explicit dunning coordination rule without discontinuing the Treatment Plan. |

## Contradictions found

No direct semantic conflict was found. Three constraints passed, two were partially
modeled, and five were absent. The gaps are meaningful and must be corrected
before the affected models can be ratified.

The most important distinction is the dunning rule: continuing existing treatment
means the clinical Treatment Plan and longitudinal care state are not cancelled or
rewritten. Freezing new fills is a separate Commerce and Operations gate. The
platform MUST represent both facts simultaneously.

## Hidden hard-coding review

- No product-specific clinical thresholds or named therapy rules were invented.
- No pharmacy SKU was used as Product identity.
- No membership payment state was used as a clinical decision.
- No Offer or experiment path could alter Protocol rules.
- The baseline did contain implied ambiguity around `Care Team`, `Provider`,
  membership dunning, and Formulation identity; these are now explicit work items.

## Disposition

Foundation Draft 0.1 remains a valid historical baseline. Corrections are additive
and will be committed after the baseline tag. The review does not promote any
chapter to `Ratified`.
