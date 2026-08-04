# 09 — Offer Engine

**Status:** Foundation Draft  
**Owner:** Commerce

## Purpose

Compose versioned commercial propositions for eligible audiences without
changing product identity or clinical policy.

## Principles

- An Offer determines presentation and commercial terms, not clinical permission.
- Eligibility dimensions remain attributable and explainable.
- Experiment assignment is stable and separate from clinical routing.
- Historical presentation and terms are reconstructable.

## Canonical Objects

Offer Definition, Offer Version, Audience Policy, Price Reference, Benefit
Reference, Promotion, Experiment Assignment, Offer Presentation, and Acceptance.

## Relationships

An Offer Version references Offering, Price, Membership, and Benefit versions. It
consumes commercial and market eligibility outcomes. Clinical eligibility may
gate downstream action but MUST NOT be reimplemented by the Offer Engine.

## Business Rules

- Presented terms bind to an immutable Offer Version and validity interval.
- Clinical requirements cannot be hidden, weakened, or rewritten by an Offer.
- Unavailable offers record reason categories suitable for authorized explanation.
- Experiment variants cannot alter clinical policy or professional decision paths.
- Acceptance does not equal an Order, Membership activation, or clinical approval.

## State Machines

Offer Version follows the shared definition lifecycle. Offer Presentation:
`eligible → presented → accepted | declined | expired | withdrawn`.

## Events

`commerce.offer.presented.v1`, `commerce.offer.accepted.v1`,
`commerce.offer.expired.v1`, and `commerce.experiment.assigned.v1`.

## Permissions

Marketing may author within approved commercial boundaries. Finance approves
financial terms. Clinical reviewers approve any wording that represents clinical
requirements, without granting Marketing authority over those requirements.

## Configuration

Audience, channel, copy references, product composition, prices, benefits,
validity, and experiments are versioned. Safety, consent, and clinical authority
boundaries are non-overridable.

## Acceptance Criteria

- A presentation can be reconstructed exactly.
- Price or copy changes create a new Offer Version.
- No experiment bypasses clinical or jurisdictional gates.
- Accepting an Offer creates no unsupported clinical fact.

## Future Extensions

Multi-product bundles, partner-funded offers, negotiated cohorts, and constrained
offer optimization with fairness monitoring.

## Anti-Patterns

Offer logic in UI code; editing live terms in place; using conversion outcome as
clinical evidence; a single eligibility boolean with no dimension or reason.

## Open Decisions

- OD-027 — OPEN — DANIEL
- OD-028 — OPEN — DANIEL
- OD-029 — OPEN — COMPLIANCE

Definitions and disposition are centralized in the
[Open Decision Register](../OPEN-DECISIONS.md).
