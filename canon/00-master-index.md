# 00 — HealthOS Canon Master Index

**Status:** Foundation Draft  
**Owner:** Platform Architecture  
**Last reviewed:** 2026-08-03

## Purpose

The Canon is the implementation-independent source of truth for HealthOS. It
defines durable platform meaning, boundaries, invariants, and contracts. It does
not prescribe user-interface details or runtime decomposition unless those
choices protect a canonical boundary.

## Normative language

The words **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, and **MAY** are
normative. Only documents marked `Ratified` are binding. `Foundation Draft`
documents guide current design and require explicit review before ratification.

## Constitutional principles

1. **Longitudinal care:** the platform optimizes for enduring health
   relationships, not isolated transactions.
2. **Configuration over code:** changeable commercial and operational policy is
   represented as governed, versioned configuration.
3. **Capabilities over features:** reusable platform capabilities serve many
   care domains and products.
4. **Clinical authority overrides commercial intent:** commercial configuration
   cannot weaken clinical policy, eligibility, or professional judgment.
5. **Everything material is versioned:** historical decisions remain
   reconstructable in their original context.
6. **HealthOS does not forget:** longitudinal history is preserved subject to
   lawful privacy, correction, retention, and erasure obligations.
7. **Intelligence assists; accountable actors decide:** automated suggestions
   are attributable, reviewable, overridable, and never obscure authority.

## Domain map

| Domain | Responsibility | Owns truth about |
| --- | --- | --- |
| Identity | People, organizations, access, consent | Who is acting and under what authority |
| Memory | Longitudinal facts, provenance, timeline | What was known, asserted, or observed and when |
| Clinical | Care relationships, protocols, decisions | What care is clinically allowed, required, and decided |
| Commerce | Offers, prices, memberships, benefits | What may be presented, purchased, or financially applied |
| Operations | Work orchestration and fulfillment | What work must happen, by whom, and by when |
| Intelligence | Derived insights and suggestions | What the system inferred and the evidence behind it |

No domain may write another domain's authoritative records directly. Cross-domain
change occurs through an owned command or a published event contract.

## Chapter registry

| Chapter | Status | Scope |
| --- | --- | --- |
| [01 — Platform Philosophy](01-platform-philosophy.md) | Foundation Draft | Constitutional intent and design tests |
| [02 — Platform Language](02-platform-language.md) | Foundation Draft | Ubiquitous language and reserved terms |
| [03 — Core Object Model](03-core-object-model.md) | Foundation Draft | Cross-domain objects, identity, provenance, and time |
| [04 — State Machines](04-state-machines.md) | Foundation Draft | Transition rules and lifecycle conventions |
| [05 — Capability Library](05-capability-library.md) | Foundation Draft | Initial capability map and ownership |
| [06 — Product System](06-product-system.md) | Foundation Draft | Patient-facing products and stable catalog identity |
| [07 — Clinical System](07-clinical-system.md) | Foundation Draft | Protocols, evaluations, and professional decisions |
| [08 — Membership Engine](08-membership-engine.md) | Foundation Draft | Access, benefits, billing, and lifecycle |
| [09 — Offer Engine](09-offer-engine.md) | Foundation Draft | Eligibility-aware presentation and pricing composition |
| [10 — Eligibility Engine](10-eligibility-engine.md) | Foundation Draft | Clinical, regulatory, operational, and commercial eligibility |
| [11 — Marketplace](11-marketplace.md) | Foundation Draft | Personalized visibility and availability explanations |
| [12 — Treatment Engine](12-treatment-engine.md) | Foundation Draft | Longitudinal treatment plans and changes |
| [13 — Pharmacy Routing](13-pharmacy-routing.md) | Foundation Draft | Product-to-fulfillment mapping and routing |
| [14 — Intelligence Platform](14-intelligence-platform.md) | Foundation Draft | Suggestions, evidence, review, and model governance |
| [15 — Security and Privacy](15-security-privacy.md) | Foundation Draft | Authorization, audit, consent, retention, and compliance |
| [16 — Analytics](16-analytics.md) | Foundation Draft | Operational and longitudinal measurement contracts |
| [17 — Decision Log](17-decision-log.md) | Active | Accepted architecture decisions |

## Ratification process

1. A chapter begins as `Foundation Draft` or `Draft`.
2. Open decisions are resolved through review or an RFC.
3. Domain owners confirm object ownership, invariants, permissions, and events.
4. Clinical and security reviewers approve affected safety or privacy rules.
5. The chapter is marked `Ratified` in both the document and this index.
6. Later semantic changes require a decision-log entry and, when cross-domain or
   constitutional, an RFC.

## Global acceptance criteria

The Canon is coherent when:

- every canonical object has exactly one owning domain;
- every material decision can be reconstructed using the version effective at
  the time and its recorded inputs;
- clinical rules cannot be bypassed by a commercial or operational path;
- all privileged actions are attributable to an actor and authority context;
- suggestions are distinguishable from facts and accountable decisions;
- implementations can change technology without changing platform meaning.

## Open decisions

- OD-001 — OPEN — COMPLIANCE
- OD-002 — OPEN — DANIEL
- OD-003 — OPEN — ENGINEERING
- OD-004 — OPEN — DANIEL

Definitions and disposition are centralized in the
[Open Decision Register](../OPEN-DECISIONS.md).
