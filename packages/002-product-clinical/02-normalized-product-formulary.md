# Normalized Product and Formulary Model

**Status:** Source population complete; authoritative review pending

## Purpose

Define and populate the normalization target for SRC-003 while preserving every
source row and separating pharmacy facts from proposed canonical groupings.

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

## Completed normalization snapshot

The 2026-08-03 snapshot contains:

- 106 preserved source product rows;
- 50 proposed stable patient-facing medication concepts;
- 69 proposed formulations;
- 101 medication pharmacy configurations after four exact duplicate rows share
  canonical configurations;
- six apparent duplicate groups, including two probable pairs left unmerged;
- 19 ambiguous groupings requiring Pharmacy decisions; and
- one multi-medication kit separated from the medication catalog.

`CFG-EP-*` is an internal canonical configuration reference, not a pharmacy-native
SKU. Every configuration maps to one or more exact SRC-003 workbook rows. Source
price fields are pharmacy facts, but they are not approved retail prices.

## Normalized outputs

- [Lossless Source Row Register](08-source-row-register.csv)
- [Normalized Product Catalog](09-normalized-product-catalog.csv)
- [Formulation and SKU Map](10-formulation-and-sku-map.csv)
- [Duplicate and Ambiguity Register](11-duplicate-and-ambiguity-register.csv)
- [Non-Medication Item Register](12-non-medication-item-register.csv)
- [Source-to-Canonical Crosswalk](13-source-to-canonical-crosswalk.csv)
- [Product Launch Readiness Matrix](14-product-launch-readiness-matrix.csv)
- [Product Normalization Review](15-product-normalization-review.md)
