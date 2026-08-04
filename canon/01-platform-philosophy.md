# 01 — Platform Philosophy

**Status:** Foundation Draft  
**Owner:** Platform Architecture

## Purpose

Define the reasoning principles used when requirements are incomplete or design
options conflict.

## Principles

### The relationship is the product

HealthOS MUST model a member's care as a continuous relationship. Encounters,
orders, prescriptions, messages, and payments are episodes within that
relationship, not substitutes for it.

### Policy is governed configuration

Rules that vary by time, market, jurisdiction, partner, or experiment SHOULD be
versioned configuration with validation, approval, effective dates, and audit
history. Code enforces the allowed shape and safety envelope of configuration.

### Clinical authority is independent

Clinical policy and professional decisions MUST remain independent from offer,
pricing, conversion, and fulfillment incentives. A commercial path MUST fail
closed when a required clinical determination is absent or negative.

### Capabilities outlive products

Platform capabilities SHOULD be named for durable work—such as medical media,
eligibility evaluation, or pharmacy routing—not for a single therapy or launch.

### History is a first-class requirement

Material records MUST be append-preserving or versioned. Corrections MUST retain
the original assertion, the correction, authorship, reason, and effective time.
Privacy and deletion requests MUST be handled through explicit policy rather
than undocumented physical deletion.

### Intelligence is bounded by authority

Generated or inferred output MUST record its provenance, confidence or limits,
and review state. It MUST NOT impersonate a clinician, silently become a clinical
fact, or execute a regulated decision without an explicitly authorized policy.

### Start cohesive; preserve seams

The first implementation SHOULD be a modular monolith with enforceable domain
boundaries. Service extraction is an operational choice, not a prerequisite for
good boundaries.

## Canonical Objects

This chapter introduces no domain objects. It governs how objects are designed.

## Relationships

The principles constrain every Canon chapter, RFC, schema, and implementation.

## Business Rules

- A new product MUST be expressible primarily by composing existing capabilities.
- A rule change MUST state whether it changes clinical policy, commercial policy,
  operational policy, or presentation.
- A historical decision MUST reference immutable or version-addressable inputs.
- Failure of a commercial capability MUST NOT corrupt clinical history.
- Clinical data use MUST be purpose-limited and authorized, including use by
  intelligence capabilities.

## State Machines

Not applicable. Lifecycle conventions are defined in Chapter 04.

## Events

Constitutional principles do not publish runtime events. Changes to ratified
principles are recorded in the decision log.

## Permissions

Ratifying or changing a constitutional principle requires designated platform,
clinical, privacy, and security approval when their authority is affected.

## Configuration

The principles themselves MUST NOT be runtime configuration.

## Acceptance Criteria

- An architecture proposal explains how it preserves history and authority.
- Product-specific requirements map to reusable capabilities.
- Variable policy is separated from fixed platform invariants.
- Human and automated authority boundaries are explicit.

## Future Extensions

- Formal architecture fitness functions.
- A machine-readable registry of invariants and owning domains.
- Jurisdiction-specific constitutional overlays that cannot weaken the base.

## Anti-Patterns

- Treating checkout completion as clinical approval.
- Encoding partner or therapy names into reusable domain objects.
- Updating a row in place when the previous value affected a decision.
- Splitting services before domain ownership is understood.
- Calling generated output a fact without validation and provenance.

## Open Decisions

- OD-005 — OPEN — ENGINEERING
- OD-006 — OPEN — DANIEL

Definitions and disposition are centralized in the
[Open Decision Register](../OPEN-DECISIONS.md).
