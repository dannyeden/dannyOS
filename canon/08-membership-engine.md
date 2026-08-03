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

Membership Plan, Membership Plan Version, Membership, Billing Arrangement,
Benefit Definition, Benefit Grant, Benefit Consumption, and Membership Change.

## Relationships

A Member enrolls in a Membership Plan Version, creating a Membership. It receives
Benefit Grants under versioned policy. Orders may consume benefits but remain
separate. Clinical access uses explicit access policy, not inferred payment state.

## Business Rules

- Medication purchases are not membership charges.
- A lapse MUST NOT erase the member timeline or clinical records.
- Benefit evaluation records the policy version and inputs used.
- Cancellation, suspension, delinquency, and expiration are distinct.
- Retroactive plan changes require explicit adjustment events.

## State Machines

Membership: `pending → active → suspended | cancelled | expired`; `suspended →
active | cancelled | expired`. Billing state is a separate lifecycle coordinated
by policy rather than collapsed into membership state.

## Events

`commerce.membership.activated.v1`, `commerce.membership.suspended.v1`,
`commerce.membership.cancelled.v1`, `commerce.benefit.granted.v1`, and
`commerce.benefit.consumed.v1`.

## Permissions

Members may manage allowed changes; authorized support may assist with attribution;
finance roles manage adjustments. No Commerce permission grants clinical access.

## Configuration

Plan terms, billing cadence, grace periods, access rules, benefit definitions,
and change policies are approved, effective-dated configuration.

## Acceptance Criteria

- Membership and medication payments reconcile independently.
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

Portal access during delinquency, proration policy ownership, and the boundary
between benefit grants and promotional credits.

