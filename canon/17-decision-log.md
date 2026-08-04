# 17 — Architecture Decision Log

**Status:** Active  
**Owner:** Platform Architecture

## Purpose

Record accepted decisions that shape multiple chapters or establish repository
governance. Detailed proposals live in RFCs; this log preserves the outcome and
its canonical consequences.

## Decision states

- **Proposed:** under review and not authoritative.
- **Accepted:** current direction and binding on drafts.
- **Superseded:** replaced by a linked decision; retained historically.
- **Rejected:** considered and not adopted.

## Decisions

### AD-001 — Separate Canon from production implementation

**Date:** 2026-08-03  
**Status:** Accepted

**Decision:** This repository contains platform Canon and architecture artifacts,
not deployable production code. Runtime applications are maintained in separate
implementation repositories.

**Rationale:** Platform meaning should survive framework, topology, vendor, and
application replacement. Separating durable contracts from implementation
prevents current code from becoming the accidental constitution.

**Consequences:**

- Backend, frontend, and deployment code are prohibited here.
- Logical schemas and interface contracts are allowed when implementation-neutral.
- Implementation repositories reference a Canon version and record intentional
  deviations.

### AD-002 — Begin with a modular monolith

**Date:** 2026-08-03  
**Status:** Accepted

**Decision:** The initial runtime architecture will be a modular monolith with
enforced domain boundaries and event-driven integration inside the process.

**Rationale:** Operational simplicity is valuable early, while explicit module
ownership preserves the option to extract services when scale, reliability,
team topology, or regulatory isolation justify it.

**Consequences:** Database ownership and module APIs remain explicit even when
deployed together. Service extraction is not an architectural milestone by
itself.

### AD-003 — Preserve historical decision context

**Date:** 2026-08-03  
**Status:** Accepted

**Decision:** Material clinical, commercial, operational, and intelligence
outcomes reference the immutable policy versions and inputs used at the time.

**Rationale:** Longitudinal care, auditability, safety review, and reproducible
analytics require reconstruction of what the platform knew and why it acted.

**Consequences:** Published definitions are immutable, corrections preserve
lineage, and current configuration cannot be used to reinterpret past decisions.

### AD-004 — Treat automated output as attributable suggestions

**Date:** 2026-08-03  
**Status:** Accepted

**Decision:** Intelligence outputs are modeled separately from facts and
accountable decisions. Acceptance or modification creates a new decision owned
by the authorized actor and domain.

**Rationale:** Reviewability and override are insufficient unless provenance and
authority remain visible in the data model.

**Consequences:** Suggestions carry evidence and generation context; downstream
systems cannot treat them as clinical decisions without an authorized transition.

### AD-005 — Coordinate membership dunning without rewriting treatment

**Date:** 2026-08-03

**Status:** Accepted

**Decision:** Membership is a universal prerequisite for treatment initiation.
During billing dunning, existing treatment continues and no new Prescriptions or
refills are issued. Membership, billing, Treatment Plan, Prescription, and Fill
remain separate lifecycles. Resumption after dunning is clinically evaluated
rather than automatically released.

**Rationale:** Financial collection state must control permitted commercial and
fulfillment actions without falsifying clinical intent or destroying longitudinal
history.

**Consequences:** A dunning transition cannot discontinue a Treatment Plan or
void historical Prescription or Treatment records. Commerce publishes billing
state; Clinical and Operations hold new Prescription/refill workflows; Clinical
history remains unchanged. This wording was clarified by the Package 003 semantic
review without collapsing the independent lifecycles.

### AD-006 — Separate Provider authority from Care Team access

**Date:** 2026-08-03

**Status:** Accepted

**Decision:** Care Team membership alone grants no prescribing authority.
Authorized Providers may reject or modify recommendations within verified scope,
and the resulting decision is independently attributable.

**Rationale:** Collaboration and decision support must not blur licensed clinical
authority or allow generated output to become an unauthorized prescription.

**Consequences:** Prescribing verifies Provider authority at decision time. The
platform preserves the recommendation, its disposition, and Provider rationale.

### AD-007 — Separate Product, Formulation, and pharmacy identity

**Date:** 2026-08-03

**Status:** Accepted

**Decision:** Product, Formulation, Strength, Package, and Pharmacy SKU are
separate identities connected through versioned mappings.

**Rationale:** Patient-facing products must survive formulation, package, and
fulfillment-partner changes without losing historical or clinical meaning.

**Consequences:** Package 002 will normalize the hierarchy from source material;
implementations cannot use a pharmacy SKU as Product identity.

### AD-008 — Establish six bounded contexts

**Date:** 2026-08-03

**Status:** Accepted

**Decision:** Identity, Memory, Clinical, Commerce, Operations, and Intelligence
are the top-level HealthOS bounded contexts. Each owns its language, objects,
rules, events, and mutation authority.

**Rationale:** Durable ownership prevents a longitudinal healthcare platform from
collapsing into shared records or product-specific workflows.

**Affected Canon:** Chapters [00](00-master-index.md), [03](03-core-object-model.md),
and [05](05-capability-library.md); Package 003
[Bounded Contexts](../packages/003-domain-rules-events/01-bounded-contexts.md).

**Supersedes:** No accepted decision. It formalizes the domain map already present
in Foundation Draft 0.1.

**Consequences:** Contexts communicate through versioned contracts and stable
references. Runtime co-location in a modular monolith does not permit direct
cross-context writes or dependence on private implementation.

### AD-009 — Federate policy ownership behind shared rule execution

**Date:** 2026-08-03

**Status:** Accepted

**Decision:** HealthOS provides shared rule-evaluation contracts and capabilities,
while each authoritative domain owns the meaning, approval, and lifecycle of its
Rule Sets.

**Rationale:** Versioned execution improves consistency and explainability, but a
central ownerless rules domain would blur clinical, commercial, operational, and
compliance authority.

**Affected Canon:** Chapters [04](04-state-machines.md),
[05](05-capability-library.md), and [10](10-eligibility-engine.md); Package 003
[Rules Engine](../packages/003-domain-rules-events/03-rules-engine.md).

**Supersedes:** The implicit assumption that a shared rules capability could own
all policies; no prior accepted decision is superseded.

**Consequences:** Rule Decisions preserve exact versions, evidence, reasons, and
owner. Composite rules cannot override a negative authoritative clinical dimension.

### AD-010 — Link material decisions to immutable Evidence Sets

**Date:** 2026-08-03

**Status:** Accepted

**Decision:** Material clinical decisions and governed Recommendations reference an
immutable manifest of the exact Evidence versions considered.

**Rationale:** Audit, appeal, explainability, protocol improvement, and historical
reconstruction require knowing what was considered, not merely the latest facts.

**Affected Canon:** Chapters [03](03-core-object-model.md),
[07](07-clinical-system.md), and [14](14-intelligence-platform.md); Package 003
[Evidence Model](../packages/003-domain-rules-events/04-evidence-model.md).

**Supersedes:** No accepted decision; extends AD-003 and AD-004 with an explicit
Evidence Set contract.

**Consequences:** Corrections append lineage and may trigger reevaluation; source
Evidence, inference, recommendation, Provider judgment, and decision remain distinct.

### AD-011 — Separate Domain Events from Health Timeline Events

**Date:** 2026-08-03

**Status:** Accepted

**Decision:** A Domain Event is an owning-domain contract fact. A Timeline Event is
a member-centered, visibility-governed Memory record referencing a material source
fact. Neither substitutes for the other.

**Rationale:** Integration contracts and longitudinal patient history have different
payload, retention, ordering, replay, and permission responsibilities.

**Affected Canon:** Chapters [02](02-platform-language.md),
[03](03-core-object-model.md), and [16](16-analytics.md); Package 003
[Event Architecture](../packages/003-domain-rules-events/05-event-architecture.md)
and [Health Timeline](../packages/003-domain-rules-events/06-health-timeline.md).

**Supersedes:** Any informal assumption that the integration event stream is itself
the patient record; no accepted decision is superseded.

**Consequences:** Memory applies versioned inclusion and visibility policy. The
Timeline remains reconstructable without becoming the owner of clinical,
commercial, operational, identity, or intelligence state.

### AD-012 — Prepare clean tenant scope without productizing multi-tenancy

**Date:** 2026-08-03

**Status:** Accepted

**Decision:** Canonical identities, definitions, events, and permissions may carry
organization or tenant scope, but Eden remains the sole initial operating context
and no tenant administration is required for the MVP.

**Rationale:** Clean scope enables future pharmacies, provider organizations,
white labels, and employer programs without premature platform machinery.

**Affected Canon:** Chapters [03](03-core-object-model.md) and
[15](15-security-privacy.md); Package 003
[Multi-Tenant Boundaries](../packages/003-domain-rules-events/08-multi-tenant-boundaries.md).

**Supersedes:** No accepted decision. It constrains, rather than mandates, future
multi-tenant implementation.

**Consequences:** Tenant configuration cannot vary canonical meaning, grant Provider
authority, weaken clinical/compliance rules, or fragment a Person's lawful
longitudinal identity.

## Open decisions

- OD-001 — OPEN — COMPLIANCE
- OD-002 — OPEN — DANIEL
- OD-057 — OPEN — ENGINEERING
- OD-058 — OPEN — DANIEL
- OD-059 — OPEN — ENGINEERING
- OD-096 — OPEN — DANIEL
- OD-098 — OPEN — ENGINEERING
- OD-100 — OPEN — DANIEL
- OD-101 — OPEN — COMPLIANCE
- OD-102 — OPEN — DANIEL
- OD-103 — OPEN — ENGINEERING
- OD-105 — OPEN — CLINICAL

Definitions and disposition are centralized in the
[Open Decision Register](../OPEN-DECISIONS.md).
