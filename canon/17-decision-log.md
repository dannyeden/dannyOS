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

## Open decisions

- Initial jurisdiction and regulatory posture.
- Named ratification authorities and domain owners.
- Canon versioning and release strategy.
- Application repository naming and dependency mechanism.

