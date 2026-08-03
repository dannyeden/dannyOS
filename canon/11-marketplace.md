# 11 — Personalized Marketplace

**Status:** Foundation Draft  
**Owner:** Commerce

## Purpose

Present a personalized, truthful view of available and unavailable care products
while preserving clinical authority and member agency.

## Principles

- Relevant unavailable products remain visible unless policy prohibits disclosure.
- Availability, eligibility, prioritization, and recommendation are distinct.
- Explanations are truthful, safe, and audience-appropriate.
- Ordering never implies clinical approval.

## Canonical Objects

Marketplace Context, Product Visibility Result, Availability Result,
Prioritization Result, Explanation, Presentation Snapshot, and Member Interaction.

## Relationships

The Marketplace reads Product and Offer versions plus federated eligibility. It
may consume Intelligence suggestions for ordering, but owns the final governed
presentation and preserves the source of each reason.

## Business Rules

- Visible unavailable products use a reason such as “Discuss with your doctor”
  when that statement accurately represents the next step.
- Sensitive reasons are generalized rather than exposed to unauthorized audiences.
- Prioritization MUST NOT manufacture eligibility or certainty.
- Every displayed term references a versioned Offer and Presentation Snapshot.
- Member interactions are not clinical consent or evidence unless explicitly captured.

## State Machines

Presentation Snapshot is immutable and time-bound. Product visibility is derived
as `visible-available`, `visible-unavailable`, or `hidden-by-policy`, with reasons.

## Events

`commerce.marketplace.presented.v1`, `commerce.marketplace.product-viewed.v1`, and
`commerce.marketplace.next-step-selected.v1` with governed data minimization.

## Permissions

Members see their authorized marketplace. Support preview requires delegated
context. Marketing cannot access clinical reasons beyond approved categories.

## Configuration

Presentation policy, ranking constraints, safe explanation mappings, and channel
layout references are versioned. Clinical results are consumed, never configured here.

## Acceptance Criteria

- Every item has traceable product, offer, eligibility, and explanation versions.
- Unavailability is not misrepresented as a technical failure.
- Intelligence ordering can be disabled without breaking the marketplace.
- Sensitive clinical facts do not enter commercial analytics payloads.

## Future Extensions

Care-plan-aware navigation, clinician-shared marketplace views, accessibility
personalization, and multilingual explanations.

## Anti-Patterns

Hiding all unavailable care; ranking by margin without constraints; exposing raw
clinical reason codes; presenting personalized ordering as medical advice.

## Open Decisions

Default visibility policy, ranking objectives and fairness review, and retention
of presentation snapshots and interaction detail.

