# Package 003 Future Multi-Tenant Boundaries

**Status:** Prepared boundary; deferred productization

## Purpose

Keep Eden's MVP simple while ensuring stable identities, ownership, configuration,
and contracts can later support multiple pharmacies, provider organizations,
white-label deployments, and employer programs.

## Minimal model

`Tenant` is a future administrative and configuration scope associated with an
Organization. It is not Person identity, clinical authority, data ownership,
brand, employer, provider organization, or pharmacy. Eden operates as the sole
initial tenant context, and Eden/store-owner governance remains authoritative;
implementations MUST NOT require tenant administration for the MVP.

## Boundaries to preserve now

- Canonical Person and clinical history do not derive identity from tenant.
- Organizations and their relationships are stable, effective-dated references.
- Every tenant-scoped definition declares scope; canonical semantic meaning cannot vary.
- Protocols, Rules, Offers, Prices, content, and routing policy support a base
  version plus explicitly governed tenant or jurisdiction overlay.
- Clinical and Compliance safety envelopes cannot be weakened by tenant overrides.
- Pharmacy and provider organizations are partners linked through contracts, not
  hard-coded Eden subtypes.
- Domain Events declare tenant/organization scope without embedding brand assumptions.
- Cross-tenant access requires explicit purpose, relationship, consent, and audit.

The MVP does not require full tenant-admin delegation, internationalization,
complex data-residency routing, cross-tenant data sharing, or tenant-defined
Clinical Protocol publishing. These remain future capabilities subject to real
partner requirements and Compliance review, not assumptions embedded now.

## Extension scenarios

| Scenario | Reused boundary | Deferred configuration |
| --- | --- | --- |
| Multiple pharmacies | Organization, Pharmacy, configuration mapping, Routing Decision | partner priority, service area, contract economics |
| Provider organizations | Organization, Role Assignment, Provider Review, routing | credential source, assignment and service levels |
| White label | Tenant scope, Product/Offer/content versions | brand, domain, theme, support and disclosure content |
| Employer program | Organization, sponsorship, Membership, Benefit Grant | eligibility roster, contribution, privacy boundary |

## Configuration precedence

Constitutional invariant → law/compliance policy → Clinical Protocol/safety rule →
platform definition → jurisdiction overlay → tenant configuration → campaign or
experiment. A lower level MUST NOT weaken or reinterpret a higher authority.

## Data and privacy

Tenant scope is a mandatory authorization dimension where applicable, but a
single Person may lawfully relate to multiple organizations or programs. Analytics,
support access, evidence use, and exports must prevent cross-tenant leakage while
preserving authorized longitudinal continuity. Tenant offboarding cannot erase
source-owned clinical history or break audit references.

## Acceptance criteria

- Eden requires no additional tenant workflow for the MVP.
- No canonical ID includes a tenant slug or brand name.
- Partner additions require configuration and contracts, not new domain objects.
- Tenant settings cannot grant Provider authority or bypass clinical rules.
- Historical outcomes retain the exact configuration scope used at the time.

## Anti-patterns

Tenant as security role; duplicated Person per brand; tenant-specific canonical
vocabulary; one pharmacy column on Product; employer access to clinical detail by
default; hard-coded Eden behavior in shared Domain Events.

## Open decisions

- OD-101 — OPEN — COMPLIANCE
