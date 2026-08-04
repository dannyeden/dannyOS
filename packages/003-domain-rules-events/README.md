# Package 003 — Domain Model, Rules Engine, and Event Architecture

**Status:** Architecture draft complete; domain review pending

**Started:** 2026-08-03

**Application code, physical schemas, and APIs:** Prohibited

## Objective

Define the implementation-independent business model from which later APIs,
databases, interfaces, intelligence, and analytics can be derived. Package 003
extends the Foundation Draft and Package 002; it does not supersede either.

## Authority and evidence

- Daniel's **HealthOS Canon — Package 003: Domain Model, Rules Engine & Event
  Architecture** brief, supplied 2026-08-03, is the package directive. Its source
  SHA-256 is `53251fb396fae112402a1f1fe972eb6d638ebb78c9e1ebccb8a16353a8d7882e`;
  the external attachment itself is not committed.
- Daniel's **Package 003 Semantic Architecture Review** directive, supplied
  2026-08-03, governs the pre-commit review. Its SHA-256 is
  `b6c5dfefd08a2610d13bb5e3fea971e9981a4723164246891f4186af48c90461`;
  the external attachment itself is not committed.
- Foundation Draft 0.1 and accepted Architecture Decisions are architectural inputs.
- Package 002 source-backed clinical and pharmacy facts remain authoritative only
  within their recorded authority boundaries.
- This package defines proposed architecture. It does not invent clinical,
  commercial, jurisdictional, or retention policy.
- Unresolved policy uses an approved `OPEN — OWNER` classification and the
  centralized Open Decision Register.

## Deliverables

1. [Bounded Contexts](01-bounded-contexts.md)
2. [Canonical Domain Objects](02-canonical-domain-objects.md)
3. [Rules Engine](03-rules-engine.md)
4. [Evidence Model](04-evidence-model.md)
5. [Event Architecture and Catalog](05-event-architecture.md)
6. [Health Timeline](06-health-timeline.md)
7. [Domain Relationship Diagrams](07-domain-relationships.md)
8. [Future Multi-Tenant Boundaries](08-multi-tenant-boundaries.md)
9. [HealthOS Design Canon](09-design-canon.md)
10. [Repository Governance Review](10-repository-governance.md)
11. [Architecture Roadmap](11-architecture-roadmap.md)
12. [Semantic Architecture Review](12-semantic-architecture-review.md)

## Package invariants

- Every canonical object, rule, event, and decision has exactly one owning domain.
- Cross-domain consumers use versioned contracts and identifiers, never direct
  writes into another domain's internals.
- The Rules Engine executes rules; the authoritative domain owns their meaning.
- Evidence is append-preserving and attributable. Corrections and lawful
  restrictions preserve lineage rather than silently rewriting clinical history.
- A Domain Event is a contract fact. A Timeline Event is a member-centered record.
- Clinical authority cannot be granted or weakened by Commerce, Operations,
  Intelligence, tenant configuration, or marketplace presentation.
- Provider judgment remains attributable; generated assistance remains distinct.

## Exit criteria

- All six bounded contexts define ownership, contracts, permissions, extension
  points, and anti-patterns.
- Every Package 003 canonical object has an explicit lifecycle and acceptance test.
- Rule decisions identify rule-set versions and evidence.
- Event contracts define versioning, idempotency, ordering, retention, and replay.
- Timeline projections can be rebuilt without becoming a second source of truth.
- Multi-tenant extension does not complicate Eden's initial operating model.
- Repository governance and the next architecture packages are explicit.
