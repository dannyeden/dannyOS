# Package 002 — Product and Clinical System

**Status:** Source Intake

**Started:** 2026-08-03

**Application code:** Prohibited

## Objective

Build the source-backed Product and Clinical model using the Eden Pharmacy
formulary and clinical documentation. Normalize products, medications,
formulations, strengths, packages, and pharmacy SKUs; extract reusable protocol,
intake, lab, visit, and medical-media definitions; and map products to those
versioned definitions without inventing missing clinical rules.

## Evidence classes

Every rule and mapping MUST carry one evidence class:

- **CONFIRMED** — directly supported by a registered source or explicit Eden decision
- **PROPOSED** — architecture offered for review, not a clinical or pharmacy fact
- **OPEN** — required information is absent, conflicting, or ambiguous; the item
  MUST use one of the five exact owner labels in the Open Decision Register

`PROPOSED` content MUST NOT be represented as `CONFIRMED` through repetition or
implementation. An open clinical rule remains assigned in the Gap Register; it
does not receive a plausible default.

## Package artifacts

1. [Source Register](01-source-register.md)
2. [File-Level Source Manifest](01a-source-manifest.md)
3. [Normalized Product and Formulary Model](02-normalized-product-formulary.md)
4. [Clinical Rule Extraction Model](03-clinical-rule-extraction.md)
5. [Product-to-Protocol Matrix](04-product-protocol-matrix.md)
6. [Gap Register](05-gap-register.md)
7. [Source Conflict Register](06-source-conflict-register.md)
8. [Reusable Clinical Module Inventory](07-reusable-clinical-modules.md)

## Entry criteria

- Foundation Draft 0.1 is preserved by commit and tag.
- Repository status and contradiction review are recorded.
- Open decisions use one approved owner class.

## Exit criteria

- Every source has provenance, version or effective date, and authority classification.
- Every formulary row resolves Medication → Formulation → Strength → Package →
  Pharmacy SKU without using SKU identity as Product identity.
- Every Product maps to an immutable Protocol Version or an explicit gap.
- Intake, lab, visit, photo, document, and medical-media requirements are reusable,
  versioned, source-linked definitions.
- Care's lab-free rule and Optimize's lab gate are represented without ambiguity.
- Conflicts and missing rules are assigned; none are silently defaulted.
- Clinical and pharmacy owners approve their authoritative normalized content.

## Current blockers

The clinical-documentation bundle is registered and under semantic review. It
contains important conflicts between Eden-approved and Beluga-source materials;
affected rules cannot be promoted until the assigned owner resolves them.

The Eden Pharmacy formulary is not present. Medication concepts can be identified
from clinical sources, but Package, partner formulation, quantity, pharmacy-native
identifier, and Pharmacy SKU normalization remain blocked.

The raw bundle is marked private/confidential and remains outside Git. The Canon
repository stores hashes, provenance, normalized summaries, and conflicts rather
than redistributing the source files.
