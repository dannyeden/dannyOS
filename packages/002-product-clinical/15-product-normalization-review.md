# Package 002 Product Normalization Review

**Source:** SRC-003

**Snapshot date:** 2026-08-03

**Status:** Complete normalization; authoritative approvals pending

## Authority boundary

SRC-003 controls current Eden Pharmacy availability, source names and categories,
formulations, strengths, package sizes, and the pharmacy pricing fields it
contains. It does not control clinical policy, membership-program placement,
state availability, patient claims, final retail pricing, or marketplace display.

The raw workbook is excluded from Git. Its SHA-256 and every exact product row are
preserved in the source manifest and lossless row register.

## Normalization results

| Measure | Count |
| --- | ---: |
| Source product rows | 106 |
| Canonical medication concepts | 50 |
| Formulations | 69 |
| Medication pharmacy configurations | 101 |
| Apparent duplicate groups | 6 |
| Ambiguous groupings | 19 |
| Non-medication items | 1 |
| Products pharmacy-available | 50 |
| Products with complete canonical mapping | 35 |
| Products with clinical source material mapped | 12 |
| Products with missing clinical source material | 38 |
| Products requiring only commercial configuration | 0 |
| Products requiring leadership decisions | 50 |
| Products with confirmed hard launch blockers | 0 |

The 101 configuration count excludes the one multi-medication kit and collapses
four exact duplicate source rows into shared configurations. Two additional
probable duplicate pairs remain separate because their source fields conflict.

## Readiness interpretation

The matrix records twelve independent readiness dimensions using only `READY`,
`INCOMPLETE`, `REVIEW_REQUIRED`, `NOT_APPLICABLE`, and `UNKNOWN`. SRC-003 makes
`pharmacy_available` `READY` for all 50 medication concepts. That fact does not
answer the other readiness questions, and an unanswered question is not itself an
authoritative hard blocker.

Thirty-five proposed canonical mappings are complete at the normalization level;
15 are `REVIEW_REQUIRED` because they participate in ambiguous source groupings.
Twelve medication concepts have clinical source material mapped: three direct
mappings are `READY` and nine require Clinical review. The other 38 are `UNKNOWN`
because no clinical source material was imported for them. Zero products require
only commercial configuration because jurisdiction, compliance, technical, or
clinical dimensions also remain unresolved. All 50 require one or more Daniel or
Eden leadership decisions. No registered authoritative source expressly identifies
any of the 50 as unable to launch, so the confirmed hard-blocker count is zero.

## Ambiguity disposition

All 19 ambiguous groupings have an explicit semantic-review classification in the
register:

| Classification | Groups | Count |
| --- | --- | ---: |
| Safe normalization | AMB-015 | 1 |
| Pharmacy review required | AMB-002–AMB-006, AMB-008–AMB-012, AMB-014, AMB-016–AMB-019 | 15 |
| Clinical review required | AMB-007, AMB-013 | 2 |
| Commercial naming decision | None | 0 |
| Preserve as separate configurations | AMB-001 | 1 |

The six apparent duplicate groups also preserve every source row. DUP-001 through
DUP-004 are safe exact normalizations in which paired rows share a canonical
configuration. DUP-005 and DUP-006 remain separate configurations pending Pharmacy
review. The source-row register and crosswalk retain all 106 unique row references;
no source row was deleted.

## Identity and lifecycle safeguard

Patient-facing Medication, Clinical Medication Concept, Formulation, Additive,
Strength, Package, Pharmacy SKU or source reference, and Pharmacy Source remain
separate identities. Ending a lower-level configuration prevents its future
selection but does not deprecate the patient-facing Medication or alter historical
Treatment, Prescription, or Fill records.

## Leadership decisions

### OD-086 — Membership-program placement

- **Why:** Pharmacy category labels do not establish Care, Optimize, or
  multi-program eligibility.
- **Affected products:** All 50 canonical medication concepts.
- **Recommended default:** Keep program placement unassigned until Clinical confirms
  applicable intake and lab policy and Commercial approves the mapping.
- **If delayed:** Program merchandising and entitlement configuration remain
  unresolved; pharmacy availability is unchanged.

### OD-087 — Patient-facing naming and marketplace visibility

- **Why:** Proposed canonical names are internal normalization labels, not approved
  consumer copy.
- **Affected products:** All 50 canonical medication concepts.
- **Recommended default:** Use a neutral canonical generic name internally and keep
  marketplace visibility off until approved; do not invent claims.
- **If delayed:** Internal fulfillment mapping can continue, but consistent patient
  display and marketplace publication cannot be finalized.

### OD-088 — Retail pricing and discount presentation

- **Why:** SRC-003 pharmacy pricing fields are authoritative source facts but are not
  approved retail offer prices.
- **Affected products:** All 50 concepts and their 101 medication configurations.
- **Recommended default:** Preserve each pharmacy price namespace and create
  separately versioned commercial Offer prices; never expose a pharmacy field as
  retail pricing by implication.
- **If delayed:** Fulfillment economics remain available for analysis, while checkout
  and offer presentation remain incomplete.

### OD-091 — Launch scope and sequence

- **Why:** The formulary establishes availability, not which category Eden should
  operationalize or promote first.
- **Affected products:** All 50 concepts across the seven source category families.
- **Recommended default:** Sequence products with mapped and reviewed clinical
  protocols first; retain the others as pharmacy-available without promoting them.
- **If delayed:** Cross-functional implementation priorities remain unclear, without
  changing pharmacy availability.

### OD-092 — Fulfillment versus marketplace eligibility

- **Why:** A pharmacy configuration may be valid for fulfillment without being an
  appropriate patient-selectable marketplace item.
- **Affected products:** All 50 concepts and all 101 medication configurations.
- **Recommended default:** Set configuration-level marketplace visibility to false
  until approved; the patient-facing Medication may exist without exposing every SKU.
- **If delayed:** Operational fulfillment can continue where otherwise authorized,
  but the marketplace cannot determine which configurations to expose.

Clinical, Pharmacy, Compliance, and Engineering decisions remain independently
owned and cannot be overridden by commercial configuration.
