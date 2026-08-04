# 08 — Membership Engine

**Status:** Foundation Draft  
**Owner:** Commerce

## Purpose

Model the commercial relationship that grants governed access and benefits while
remaining separate from clinical care and medication purchasing.

## Principles

- Membership, billing, benefits, and clinical eligibility are independent.
- Membership does not guarantee treatment, prescription, or fulfillment.
- Benefits are explicit, versioned entitlements.
- Lifecycle changes preserve earned and consumed history.

## Canonical Objects

Membership Plan, Membership Plan Version, Membership, Billing Option, Billing
Arrangement, Benefit Definition, Benefit Grant, Benefit Consumption, and
Membership Change.

## Relationships

A Member enrolls in a Membership Plan Version, creating a Membership. It receives
Benefit Grants under versioned policy. A member selects one of the Plan Version's
Billing Options to create a Billing Arrangement. Orders may consume benefits but
remain separate. Clinical access uses explicit membership policy, not an inferred
payment boolean.

## Business Rules

- Medication purchases are not membership charges.
- Membership is a universal prerequisite for treatment initiation. It is
  necessary but not sufficient: clinical eligibility and Provider authority
  remain independent requirements.
- A Membership Plan Version MAY expose multiple approved Billing Options. Each
  option and the member's selected option are versioned and preserved.
- A lapse MUST NOT erase the member timeline or clinical records.
- Benefit evaluation records the policy version and inputs used.
- Cancellation, suspension, delinquency, and expiration are distinct.
- During billing dunning, the existing Treatment Plan and clinical history
  continue without mutation, while new medication Fills are frozen. Dunning MUST
  NOT be represented as clinical discontinuation.
- Retroactive plan changes require explicit adjustment events.

## State Machines

Membership: `pending → active → suspended | cancelled | expired`; `suspended →
active | cancelled | expired`. Billing state is a separate lifecycle coordinated
by policy rather than collapsed into membership state. Billing Arrangement:
`current → payment-due → dunning → current | exhausted`; membership remains an
independent state while dunning policy coordinates fulfillment gates.

## Events

`commerce.membership.activated.v1`, `commerce.membership.suspended.v1`,
`commerce.membership.cancelled.v1`, `commerce.benefit.granted.v1`, and
`commerce.benefit.consumed.v1`. Billing publishes
`commerce.billing-arrangement.entered-dunning.v1` and
`commerce.billing-arrangement.recovered.v1`.

## Permissions

Members may manage allowed changes; authorized support may assist with attribution;
finance roles manage adjustments. No Commerce permission grants clinical access.

## Configuration

Plan terms, Billing Options, cadence, timing, grace periods, access rules, benefit
definitions, and change policies are approved, effective-dated configuration.

## Acceptance Criteria

- Membership and medication payments reconcile independently.
- Treatment cannot initiate before the membership prerequisite is satisfied.
- Dunning preserves existing treatment while preventing new Fills.
- Multiple Billing Options can be configured without application code changes.
- Benefit decisions reproduce from preserved versions.
- Membership loss does not destroy longitudinal history.
- Clinical decisions remain valid or expire by clinical policy, not billing state.

## Future Extensions

Household plans, employer sponsorship, regional plans, benefit wallets, and
portable membership across partner care networks.

## Anti-Patterns

`isSubscriber` as the membership model; bundling medication capture with dues;
rewriting prior benefits after a plan edit; treating membership as clinical consent.

## Open Decisions

- OD-024 — OPEN — DANIEL
- OD-025 — OPEN — DANIEL
- OD-026 — OPEN — DANIEL
- OD-060 — OPEN — DANIEL
- OD-061 — OPEN — CLINICAL

Definitions and disposition are centralized in the
[Open Decision Register](../OPEN-DECISIONS.md).
