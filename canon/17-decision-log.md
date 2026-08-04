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
During billing dunning, existing treatment continues and new medication Fills
freeze. Membership, billing, Treatment Plan, Prescription, and Fill remain
separate lifecycles.

**Rationale:** Financial collection state must control permitted commercial and
fulfillment actions without falsifying clinical intent or destroying longitudinal
history.

**Consequences:** A dunning transition cannot discontinue a Treatment Plan or
void a Prescription. Commerce publishes billing state; Operations enforces the
new-Fill hold; Clinical history remains unchanged.

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

## Open decisions

- OD-001 — OPEN — COMPLIANCE
- OD-002 — OPEN — DANIEL
- OD-057 — OPEN — ENGINEERING
- OD-058 — OPEN — DANIEL
- OD-059 — OPEN — ENGINEERING

Definitions and disposition are centralized in the
[Open Decision Register](../OPEN-DECISIONS.md).
