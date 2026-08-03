# 06 — Product System

**Status:** Foundation Draft  
**Owner:** Commerce

## Purpose

Define stable patient-facing care products that compose clinical, commercial,
and operational capabilities without inheriting their implementation details.

## Principles

- A Product is not an Offer, Protocol, Prescription, or pharmacy SKU.
- Product identity remains stable while formulations, partners, and prices evolve.
- Products compose versioned capabilities and policies.
- Deprecation preserves historical identity and references.

## Canonical Objects

| Object | Meaning |
| --- | --- |
| Product | Stable patient-facing care concept |
| Product Version | Immutable effective-dated clinical and experience composition |
| Product Variant | Explicit option within a product, not a fulfillment SKU |
| Product Availability Policy | Versioned market and channel availability constraints |
| Product Fulfillment Requirement | Product-owned requirements consumed by Operations routing |

## Relationships

A Product Version references Protocol Versions and required capabilities. An
Offering references a Product Version. Operations-owned fulfillment mappings may
resolve to partner SKUs, but the Product does not expose or own those SKUs.

## Business Rules

- Publishing requires clinical approval of all referenced clinical configuration.
- A commercial actor MUST NOT change protocol requirements through a Product.
- Existing outcomes retain the exact Product Version used.
- Product retirement blocks new initiation but does not erase ongoing care.
- Formulation substitution requires explicit clinical and fulfillment policy.

## State Machines

Product Version: `draft → approved → active → superseded | retired`; `withdrawn`
is terminal before activation. Product lifecycle is independent of Offering and
Protocol lifecycles.

## Events

`commerce.product.version-published.v1`, `commerce.product.activated.v1`,
`commerce.product.retired.v1`, and
`operations.fulfillment-mapping.changed.v1`.

## Permissions

Commerce authors product composition; Clinical approves clinical references;
Operations approves fulfillment feasibility. Publishing requires separation of
duties for safety-impacting changes.

## Configuration

Names, descriptions, market availability, capability composition, and mappings
are versioned configuration. Clinical safety constraints are referenced, not
copied into commercial configuration.

## Acceptance Criteria

- A pharmacy or price can change without changing Product identity.
- Historical care resolves the original Product Version.
- No Product publication bypasses required clinical approval.
- New care areas reuse capabilities before adding product-specific behavior.

## Future Extensions

International product localization, device and diagnostic products, product
families, and regulated labeling profiles.

## Anti-Patterns

One product row per pharmacy SKU; embedding price in Product; therapy-specific
code paths; deleting retired products; copying Protocol rules into Product JSON.

## Open Decisions

The boundary between Product Variant and treatment option; approval workflow for
non-clinical content; compatibility rules for changing capability composition.
