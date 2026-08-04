# Normalized Product and Formulary Model

**Status:** Proposed architecture; source population blocked

## Purpose

Define the normalization target for SRC-003 without asserting any medication,
formulation, strength, package, or SKU data before the formulary is received.

## Identity hierarchy

| Object | Owner | Identity and meaning |
| --- | --- | --- |
| Product | Commerce | Stable patient-facing care product |
| Medication | Clinical | Canonical therapeutic or medicinal concept independent of vendor |
| Formulation | Clinical | Ingredient composition, route, and dosage form |
| Strength | Clinical | Normalized amount and unit associated with a Formulation |
| Package | Operations | Dispensable quantity, unit, and packaging presentation |
| Pharmacy SKU | Operations / pharmacy source | Pharmacy-specific catalog or inventory identity for a Package |
| Product Fulfillment Mapping | Operations | Effective-dated mapping from Product and clinical authorization to eligible Pharmacy SKUs |

Ownership is proposed pending Clinical and Pharmacy review. No shared identifier
may substitute for two levels of the hierarchy.

## Required normalized fields

### Medication

Stable ID, canonical name, source identifiers, active ingredients where supplied,
status, provenance, and effective interval.

### Formulation

Stable ID, Medication reference, ingredient composition, dosage form, route,
release characteristics where supplied, status, provenance, and effective interval.

### Strength

Stable ID, Formulation reference, normalized numerator and denominator values and
units, source display value, provenance, and effective interval. Units MUST NOT be
collapsed into an unparsed display string.

### Package

Stable ID, Strength reference, quantity, quantity unit, container or package type
where supplied, source description, provenance, and effective interval.

### Pharmacy SKU

Stable ID, Pharmacy Organization reference, pharmacy-native identifier, Package
reference, source status, service or jurisdiction constraints if supplied,
provenance, and effective interval.

## Mapping invariants

- A Product MUST NOT use a Pharmacy SKU as its identifier.
- A Formulation or Strength change creates a distinct identity or versioned mapping;
  it is not a label edit.
- Pharmacy-native descriptions are preserved as source values alongside normalized data.
- Unparseable or conflicting values enter the Gap Register; they are not guessed.
- Mappings identify source, reviewer, approval, effective interval, and supersession.
- Product continuity is preserved when a pharmacy, Package, or SKU changes.

## Normalization workflow

1. Register the exact formulary source and effective date.
2. Preserve every source row with a stable locator.
3. Extract pharmacy-native identifiers and descriptions without transformation.
4. Normalize Medication, Formulation, Strength, and Package independently.
5. Create Pharmacy SKU records and candidate mappings.
6. Record duplicates, ambiguities, and conflicts in the Gap Register.
7. Obtain Pharmacy review before marking mappings `CONFIRMED`.
8. Publish an immutable normalized snapshot; later changes supersede it.

## Current state

No formulary records have been created. SRC-003 is missing.
