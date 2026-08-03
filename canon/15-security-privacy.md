# 15 — Security and Privacy

**Status:** Foundation Draft  
**Owner:** Security and Privacy, federated with domain owners

## Purpose

Define platform-wide controls that preserve confidentiality, integrity,
availability, member agency, and accountable use across longitudinal health data.

## Principles

- Deny by default and grant least privilege for a defined purpose.
- Authentication, authorization, consent, and clinical relationship are distinct.
- Sensitive actions and disclosures are attributable and auditable.
- Privacy obligations can constrain longitudinal retention.
- Security controls apply equally to humans, services, partners, and intelligence.

## Canonical Objects

| Owner | Objects |
| --- | --- |
| Identity | Principal, Credential, Session, Actor Context, Role Assignment, Delegation, Authorization Policy Version, Consent Grant, Purpose of Use, and Access Decision |
| Memory | Data Classification Assignment, Audit Event, and disclosure lineage |
| Operations | Retention Policy, Legal Hold, Privacy Request, and security incident workflow |

## Relationships

Identity establishes principals and actor context. Domain services authorize each
action using centrally governed policy inputs and domain ownership. Memory retains
provenance and audit references without becoming a universal access bypass.

## Business Rules

- Every access decision identifies principal, actor, action, resource, purpose,
  policy version, context, and outcome.
- Service credentials are workload-specific, rotatable, and non-human.
- Break-glass access is explicit, justified, time-bound, alerted, and reviewed.
- Consent does not replace other required authority, and revocation affects future use.
- Correction, restriction, retention, and erasure follow explicit policy with lineage.
- Secrets and raw sensitive content MUST NOT be placed in logs or event metadata.

## State Machines

Consent Grant: `proposed → active → revoked | expired | superseded`. Privacy Request:
`received → verifying → assessing → executing → completed | denied | cancelled`,
with reason, authority, and evidence at each transition.

## Events

`identity.consent.activated.v1`, `identity.consent.revoked.v1`,
`identity.break-glass-access.invoked.v1`, `operations.privacy-request.completed.v1`,
and governed Audit Events that are not indiscriminately broadcast.

## Permissions

Authorization combines role and attributes including organization, relationship,
purpose, jurisdiction, sensitivity, consent, assurance, and time. Administrative
access does not imply clinical access.

## Configuration

Policies, classifications, retention schedules, session controls, and partner
scopes are versioned and approved. Configuration cannot disable mandatory audit,
separation of duties, or protected control planes.

## Acceptance Criteria

- Privileged access is attributable and reviewable.
- Revocation and policy changes take effect predictably.
- Data exports and partner disclosures are scoped and auditable.
- Recovery, incident response, and continuity controls are testable.
- Longitudinal retention and privacy obligations resolve through explicit policy.

## Future Extensions

Fine-grained consent, regional policy overlays, confidential computing, member
access transparency, and automated partner assurance.

## Anti-Patterns

Role-only authorization; shared administrator accounts; consent as a universal
boolean; PHI in logs; immutable retention claims that ignore lawful privacy duties.

## Open Decisions

Initial legal jurisdictions, control framework mapping, data-classification levels,
retention authorities, and member-facing access transparency.
