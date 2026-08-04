# 07 — Clinical System

**Status:** Foundation Draft  
**Owner:** Clinical

## Purpose

Define independent clinical policy, evidence gathering, professional evaluation,
and attributable decisions across care domains.

## Principles

- Clinical policy is independent of products and commercial presentation.
- Licensed professionals retain authority for decisions requiring that authority.
- Evidence, suggestions, and decisions are distinct records.
- Unmet requirements fail closed and remain explainable.
- Protocol versions and decision context are immutable and reconstructable.

## Canonical Objects

| Object | Meaning |
| --- | --- |
| Protocol Definition and Version | Governed clinical policy and its immutable release |
| Clinical Requirement | Evidence, action, qualification, or authority required by protocol |
| Questionnaire Definition and Response Set | Versioned questions and attributable answers |
| Evaluation | Evidence-gathering process under a specific Protocol Version |
| Clinical Decision | Attributable professional determination with rationale and scope |
| Contraindication Finding | Evidence-linked condition affecting allowed care |
| Follow-up Plan | Required monitoring and review cadence |

## Relationships

Products reference Protocol Versions. Evaluations execute a Protocol Version and
assemble evidence. Suggestions may support an Evaluation. A Clinical Decision
may authorize or reject a Treatment Plan or Prescription; Commerce only consumes
the resulting permitted facts.

## Business Rules

- Protocol publication requires designated clinical authority.
- A decision records decision maker, credential context, jurisdiction, protocol
  version, evidence set, rationale, scope, and time.
- Missing required evidence MUST NOT be interpreted as negative evidence.
- Overrides require explicit permission, reason, and policy-defined boundaries.
- An authorized Provider MAY reject or modify a system recommendation within
  verified scope. The resulting decision records the Provider, rationale, and
  recommendation disposition; this authority does not bypass a non-overridable
  Protocol constraint.
- Care Team membership alone MUST NOT authorize prescribing. A Prescription
  requires separately verified Provider authority, credential, jurisdiction, and
  scope at decision time.
- Commercial eligibility never converts a negative clinical determination.
- Amendments and corrections preserve the original clinical record.

## State Machines

Evaluation: `created → collecting-evidence → ready-for-review → in-review →
completed | unable-to-complete | cancelled`. A completed Evaluation produces a
separate Clinical Decision; reopening creates a linked new review episode.

## Events

`clinical.evaluation.ready-for-review.v1`, `clinical.evaluation.completed.v1`,
`clinical.decision.recorded.v1`, `clinical.decision.amended.v1`, and
`clinical.protocol.version-activated.v1`.

## Permissions

Access and transitions depend on professional role, credential and jurisdiction
context, care relationship, purpose of use, and organization authority. Support
roles cannot impersonate clinical authority.

## Configuration

Protocols, questionnaires, evidence requirements, review routing, and follow-up
rules are clinically governed versioned configuration. Commercial configuration
cannot edit or suppress them.

## Acceptance Criteria

- A decision is reproducible from preserved policy and evidence references.
- Authority and jurisdiction are verified at decision time.
- Recommendation overrides are attributable to the authorized Provider.
- Care-team actors without verified Provider authority cannot prescribe.
- Missing, conflicting, and stale evidence are distinguishable.
- Product changes cannot silently alter active clinical policy.

## Future Extensions

Collaborative decisions, specialty consultations, device evidence, external
record reconciliation, and standards-based exchange profiles.

## Anti-Patterns

An `approved` boolean without authority; questionnaire completion as approval;
protocols embedded in products; generated suggestions written as diagnoses;
commercial staff editing clinical requirements.

## Open Decisions

- OD-020 — OPEN — COMPLIANCE
- OD-021 — OPEN — CLINICAL
- OD-022 — OPEN — CLINICAL
- OD-023 — OPEN — CLINICAL
- OD-061 — OPEN — CLINICAL
- OD-062 — OPEN — CLINICAL
- OD-063 — OPEN — CLINICAL

Definitions and disposition are centralized in the
[Open Decision Register](../OPEN-DECISIONS.md).
