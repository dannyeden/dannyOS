# 12 — Treatment Engine

**Status:** Foundation Draft  
**Owner:** Clinical

## Purpose

Represent longitudinal, clinician-governed treatment plans and their monitored
evolution independently of products, orders, and fulfillment partners.

## Principles

- A Treatment Plan is longitudinal and versioned.
- Plan intent is distinct from prescription and fulfillment execution.
- Changes preserve rationale, evidence, and clinical authority.
- Monitoring and follow-up are explicit obligations.

## Canonical Objects

Treatment Plan, Treatment Plan Version, Treatment Goal, Intervention,
Monitoring Requirement, Follow-up Schedule, Plan Change, and Treatment Episode.

## Relationships

A Clinical Decision authorizes a Treatment Plan Version. Interventions may
reference Products or non-product care. Prescriptions and operational workflows
implement portions of the plan without owning its clinical intent.

## Business Rules

- Only authorized clinical actors approve safety-impacting plan changes.
- Each active intervention states goal, instructions, effective interval, and monitoring.
- A fulfillment failure does not silently cancel clinical intent; it creates an exception.
- Stopping, pausing, replacing, and completing treatment are distinct changes.
- Current plan views are derived from preserved versions and changes.

## State Machines

Treatment Plan: `proposed → active → paused | completed | discontinued | superseded`;
`paused → active | discontinued | superseded`. Each transition records rationale.

## Events

`clinical.treatment-plan.activated.v1`, `clinical.treatment-plan.changed.v1`,
`clinical.treatment-plan.paused.v1`, and `clinical.monitoring.overdue.v1`.

## Permissions

Clinical authority governs approval and change. Members may report outcomes,
preferences, and adherence without directly rewriting clinical instructions.

## Configuration

Protocol-owned plan templates, monitoring rules, and follow-up cadence are
versioned. Patient-specific plans are attributable records, not reusable config.

## Acceptance Criteria

- The plan at any historical time can be reconstructed.
- Clinical intent survives pharmacy or product mapping changes.
- Required monitoring produces actionable work and escalation.
- Member-reported facts remain distinct from clinician decisions.

## Future Extensions

Shared decision-making records, multi-specialty plans, device-driven monitoring,
goal progress, and external care-plan exchange.

## Anti-Patterns

Using active prescriptions as the treatment plan; overwriting dosage history;
closing treatment when an order fails; product-specific treatment tables.

## Open Decisions

Plan granularity across conditions, collaborative approval rules, and ownership of
adherence assertions and interventions.

