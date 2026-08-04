# Repository Status

**Current package:** Package 003 — Domain Model, Rules Engine, and Event Architecture

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

Architecture decisions AD-001 through AD-012 are accepted within the current
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
- The Eden Pharmacy formulary has been normalized into proposed canonical
  concepts and source-linked configurations; Pharmacy, Clinical, Commercial,
  and Compliance approvals remain open.
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
4. Daniel's **HealthOS Canon — Package 003: Domain Model, Rules Engine & Event
   Architecture** brief supplied on 2026-08-03; the Package 003 README records its
   hash and authority boundary while the external attachment remains outside Git.
5. Daniel's **Package 003 Semantic Architecture Review** directive supplied on
   2026-08-03; its hash and resulting correction review are recorded in Package 003.

The Beluga clinical-documentation bundle and authoritative Eden Pharmacy
formulary were received after the 0.1 baseline and are registered in Package 002
with file-level hashes and authority classifications. Raw private sources remain
outside Git. No proposed canonical grouping or extracted clinical rule becomes
foundational until its authoritative owner approves it.

## Intentionally out of scope

- Production application, frontend, backend, infrastructure, and deployment code
- Production database migrations and ORM schemas
- Invented clinical thresholds, contraindications, dosing, lab panels, or visit rules
- Final jurisdiction-specific legal or regulatory claims
- Vendor selection and detailed runtime topology
- Final UI, copy, analytics metrics, or model selection

## Active package

**Package 003 — Domain Model, Rules Engine, and Event Architecture** extends the
Foundation and Package 002 with:

- formal Identity, Memory, Clinical, Commerce, Operations, and Intelligence
  bounded contexts;
- independent specifications for canonical business objects and lifecycles;
- federated policy ownership behind versioned Rule Set evaluation;
- immutable Evidence and Evidence Set semantics;
- Domain Event contracts, catalog, delivery, compatibility, retention, and replay;
- a member-centered Health Timeline distinct from integration transport;
- clean future multi-tenant boundaries without MVP administration;
- a patient experience Design Canon, repository governance review, and roadmap.

Package 002 remains the source-backed Product and Clinical workstream. Its active
clinical conflicts, missing documentation, program placement, protocol mappings,
and pharmacy configuration decisions move into the next clinical-resolution phase;
Package 003 does not resolve them by architectural inference.

No application code begins until the core object, product, protocol,
questionnaire, lab, membership, care-plan, prescription-review, and commercial
offer/version models are stable enough for implementation review.
