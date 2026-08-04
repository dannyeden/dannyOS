# Repository Status

**Current package:** Package 001 — Canon Foundation

**Baseline:** Foundation Draft 0.1

**Baseline commit:** `cae2459`

**Baseline tag:** `canon-v0.1`

**Status date:** 2026-08-03

## Authoritative now

Foundation Draft 0.1 is the preserved initial architecture baseline. It is the
current source of truth for design work, but it is not yet ratified Canon.

The following sponsor constraints are authoritative inputs for review and Package
002 even where Foundation Draft 0.1 did not yet encode them fully:

- Membership is a universal prerequisite for initiating treatment.
- Care is lab-free.
- Optimize is lab-gated.
- Membership supports multiple configurable billing options.
- Authorized providers may override recommendations; the override is attributable.
- Care-team authority alone cannot prescribe.
- Treatment history is never overwritten.
- Product, formulation, and pharmacy SKU are separate identities.
- Offer configuration cannot override clinical rules.
- During membership dunning, existing treatment continues and new fills freeze.

Architecture decisions AD-001 through AD-007 are accepted within the current
Foundation Draft and guide current work. The six-domain ownership model, clinical-over-
commercial authority rule, versioning model, and distinction among facts,
suggestions, and decisions are current working constraints.

## Provisional

- Every Canon chapter remains `Foundation Draft` until semantic review and
  explicit ratification.
- Object names and boundaries may change as source-backed Product and Clinical
  models are developed.
- Jurisdiction, compliance posture, named domain owners, ratification authority,
  identifier format, and external schema compatibility remain open.
- Product-specific mappings have not yet been normalized from pharmacy and
  clinical source documents.
- No technology-specific physical data model or API contract is canonical.

All unresolved decisions are tracked in [OPEN-DECISIONS.md](OPEN-DECISIONS.md)
with one accountable owner class.

## Source materials

Foundation Draft 0.1 was informed by:

1. The **HealthOS Platform Canon & Architecture Project** brief supplied by
   Daniel on 2026-08-03.
2. Daniel's Foundation Draft 0.1 review directive supplied on 2026-08-03,
   including the ten authoritative constraints listed above.
3. Architectural synthesis performed from those materials; where the sources
   were silent, the draft records proposed architecture rather than invented
   clinical rules.

The Eden Pharmacy formulary and clinical documentation are required sources for
Package 002. They are not part of the 0.1 baseline and must be registered with
provenance before their rules become foundational.

## Intentionally out of scope

- Production application, frontend, backend, infrastructure, and deployment code
- Production database migrations and ORM schemas
- Invented clinical thresholds, contraindications, dosing, lab panels, or visit rules
- Final jurisdiction-specific legal or regulatory claims
- Vendor selection and detailed runtime topology
- Final UI, copy, analytics metrics, or model selection

## Next package

**Package 002 — Product and Clinical System** will:

- normalize formulary products, medications, formulations, strengths, packages,
  and pharmacy SKUs;
- register every pharmacy and clinical source with provenance;
- extract reusable intake, lab, synchronous-visit, photo, document, and medical-
  media requirements;
- create initial versioned protocol templates;
- map products to protocols, labs, intakes, and capabilities;
- separate confirmed rules from proposed architecture; and
- produce an explicit Eden decision gap register without inventing clinical rules.

No application code begins until the core object, product, protocol,
questionnaire, lab, membership, care-plan, prescription-review, and commercial
offer/version models are stable enough for implementation review.
