# 13 — Pharmacy Routing

**Status:** Foundation Draft  
**Owner:** Operations

## Purpose

Route authorized prescriptions and fulfillment requests among eligible pharmacies
without coupling patient-facing products to partner-specific behavior.

## Principles

- Routing occurs after and within clinical authorization.
- Patient-facing identity is stable across pharmacy changes.
- Eligibility, selection, submission, and fulfillment are separate facts.
- Routing decisions are explainable, versioned, and reversible where allowed.

## Canonical Objects

Pharmacy Partner, Partner Capability Version, Fulfillment Mapping, Routing Policy
Version, Routing Request, Candidate Assessment, Routing Decision, Partner
Assignment, and Fulfillment Exception.

## Relationships

The engine consumes Prescription scope, Product fulfillment mappings, member and
jurisdiction context, inventory or capability signals, and operational policy. It
issues a Partner Assignment; the pharmacy owns its external fulfillment lifecycle.

## Business Rules

- Only candidates compatible with the prescription and jurisdiction are evaluated.
- Cost or margin MUST NOT override clinical, legal, or safety constraints.
- A reroute preserves previous attempts and prevents duplicate active fulfillment.
- Partner SKU mappings are versioned and never redefine Product identity.
- Submission uses idempotency and reconciliation controls.

## State Machines

Routing Request: `created → evaluating → assigned | no-route | failed | cancelled`.
Assignment: `pending-submission → submitted → acknowledged | rejected | cancelled | superseded`.

## Events

`operations.pharmacy-route.assigned.v1`, `operations.pharmacy-route.unavailable.v1`,
`operations.fulfillment.submitted.v1`, and `operations.fulfillment.exception-raised.v1`.

## Permissions

Operations manages partners and routing within approved policy. Clinical data
shared with a partner is minimized to authorized fulfillment purpose.

## Configuration

Capabilities, mappings, priority constraints, service areas, and retry policies
are versioned. Prescription scope and jurisdictional constraints are not overridable.

## Acceptance Criteria

- Pharmacy changes do not change Product or Treatment Plan identity.
- Every assignment includes evaluated candidates and reason codes.
- Retries and reroutes cannot create duplicate fulfillment.
- Partner disclosures are purpose-limited and auditable.

## Future Extensions

Inventory-aware routing, split fulfillment, specialty pharmacies, international
networks, and member preference within permitted candidates.

## Anti-Patterns

Pharmacy ID on Product; routing by price alone; destructive reassignment; sending
full clinical records when a minimal fulfillment payload suffices.

## Open Decisions

Candidate scoring governance, real-time capability freshness, reroute consent, and
source of truth for partner formulary and jurisdiction data.

