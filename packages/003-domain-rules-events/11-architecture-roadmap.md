# Package 003 Architecture Roadmap

**Status:** Proposed sequence

## Sequencing principle

Resolve semantic and authority risk before implementation detail. The next work
should be a small decision packet and source-backed clinical resolution, not another
round of pharmacy-row normalization or production application code.

## Phase 1 — Package 002 clinical resolution

- Resolve the active source conflicts and record authoritative outcomes.
- Obtain source material or explicit Clinical decisions for the 38 medications
  currently lacking imported clinical documentation.
- Approve Product-to-Protocol, intake, lab, visit, media, and Provider-workflow mappings.
- Decide Care, Optimize, and multi-program placement without inferring from pharmacy categories.
- Classify pharmacy configurations as equivalent, preferred, fallback, prohibited,
  or still under review without deleting source rows.

Output: a concise Clinical and Leadership Decision Packet organized by policy,
affected Product concepts, recommendation, evidence, and consequences—not 106-row review.

## Phase 2 — Package 003 ratification review

- Review six bounded-context ownership and the canonical object catalog.
- Ratify Rules Engine ownership, Evidence semantics, Domain Event/Timeline distinction,
  and multi-tenant safety envelope.
- Resolve the Package 003 decisions OD-096, OD-098, OD-100 through OD-103, and
  OD-105, plus linked existing governance/evidence decisions, or record explicit deferrals.
- Reconcile Package 003 terms into the Platform Language and Capability Library only
  after review; do not rewrite Foundation history.

Output: accepted RFC/Decision entries and a Package 003 semantic review.

## Phase 3 — Contract definitions

Create implementation-neutral, machine-readable candidates only after semantics are
stable:

- canonical identifier and vocabulary registry;
- initial Domain Event envelope and highest-value event contracts;
- Rule Definition, Rule Decision, and Evidence Set contracts;
- Timeline Event contract and audience visibility policy;
- conformance examples for membership initiation/dunning, clinical eligibility,
  Provider override, prescription, Fill, and shipment.

No database, API, ORM, or runtime vendor is selected in this phase.

## Phase 4 — Logical application architecture

Map accepted capabilities into modular-monolith modules, owned persistence
boundaries, process managers, transaction/publication patterns, authorization
enforcement, and implementation-repository acceptance tests. Define APIs and
physical schemas only here, derived from Canon contracts.

## Phase 5 — Initial implementation readiness

Production work begins when these models are reviewed enough to prevent hidden
hard-coding:

- identity and actor authority;
- Product, Offer, Price, and Membership;
- Protocol, Questionnaire, Lab, Evidence, and Rules;
- Care Plan, Treatment Plan, Provider Review, and Prescription;
- Order, Fill, pharmacy routing, Shipment, and dunning coordination;
- Domain Event, Timeline, audit, and permission contracts.

## Later packages

| Package | Scope | Entry dependency |
| --- | --- | --- |
| 004 | Clinical Resolution and Product Mapping | Authoritative Clinical/leadership review |
| 005 | Canonical Contracts and Conformance | Package 003 semantic approval |
| 006 | Logical Security, Privacy, and Tenant Controls | Compliance decisions and contract candidates |
| 007 | Analytics Semantics and Outcome Model | Stable events, Timeline, and clinical outcomes |
| 008 | Implementation Architecture | Accepted contracts and implementation repository |

## Roadmap guardrails

- Do not use schema work to settle unresolved business meaning.
- Do not treat Package completion as clinical approval.
- Do not generalize multi-tenancy beyond clean boundaries until a second tenant use
  case is real.
- Do not expand AI autonomy; expand evidence, review, safety, and explanation first.
- Do not begin application code while critical object, rule, event, and authority
  semantics remain hidden or contradictory.
