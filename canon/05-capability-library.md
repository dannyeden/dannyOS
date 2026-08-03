# 05 — Capability Library

**Status:** Foundation Draft  
**Owner:** Platform Architecture

## Purpose

Define reusable platform capabilities and their domain ownership before products
or implementation components are designed.

## Principles

- Capabilities describe durable business ability, not screens or services.
- Each capability has one owning domain and explicit consumers.
- Products compose capabilities; they do not fork them.
- A capability may begin in a modular monolith and later move without changing
  its canonical contract.

## Canonical Objects

| Object | Meaning |
| --- | --- |
| Capability Definition | Named business ability, owner, responsibilities, invariants, inputs, and outcomes |
| Capability Contract | Versioned commands, queries, events, and policy boundaries exposed to consumers |
| Capability Composition | Governed arrangement of capabilities for a product, protocol, or workflow |

## Relationships

Capabilities belong to one domain, may consume contracts from others, and may be
composed by product and workflow definitions. Owning a capability does not grant
authority over consumed facts.

## Business Rules

- A new product requirement MUST first map to this library.
- A new capability requires a clear owner and explanation of why extending an
  existing capability is insufficient.
- Capability contracts MUST separate commands, facts, and suggestions.
- Clinical capabilities define the clinical safety envelope; Commerce cannot
  override it through composition.

## Initial capability map

### Identity

- Person and account resolution
- Authentication and session assurance
- Role, delegation, and organization authority
- Consent and purpose-of-use management
- Care-team relationship management

### Memory

- Longitudinal member timeline
- Assertion and correction management
- Provenance and source-artifact registry
- Medical media management
- Record import, reconciliation, and export

### Clinical

- Protocol authoring and versioning
- Questionnaire and assessment execution
- Clinical eligibility evaluation
- Provider evaluation and decision capture
- Lab ordering and result reconciliation
- Treatment planning and monitoring
- Prescription lifecycle management
- Renewal and follow-up policy execution

### Commerce

- Product catalog management
- Offering and market availability
- Offer and pricing composition
- Membership and benefit management
- Order orchestration
- Payment authorization, capture, refund, and reconciliation
- Experiment assignment and contribution measurement

### Operations

- Workflow and work-item orchestration
- Care-team routing and escalation
- Partner and provider-group routing
- Pharmacy routing and fulfillment coordination
- Notification preference and delivery orchestration
- Exception, incident, and reconciliation management

### Intelligence

- Evidence assembly and retrieval
- Suggestion generation and review
- Member prioritization and next-best-focus support
- Provider and care-team decision support
- Model, prompt, and policy version governance
- Quality, safety, drift, and outcome monitoring

## State Machines

Each capability owns the lifecycles of its aggregates and follows Chapter 04.
Cross-capability workflows are process managers that react to domain events and
issue authorized commands.

## Events

Every capability contract declares the facts it publishes and consumes. Event
payloads MUST avoid leaking sensitive data that consumers can resolve by an
authorized query.

## Permissions

Every capability declares actor types, allowed actions, data sensitivity,
purpose-of-use requirements, and break-glass behavior. UI visibility is not an
authorization control.

## Configuration

Each capability declares which policies are configurable, their schema,
validation, approvers, effective dating, rollback or supersession strategy, and
the safety envelope configuration cannot exceed.

## Acceptance Criteria

- Every planned feature maps to one or more owned capabilities.
- Capability overlap and shared write ownership are absent.
- Clinical, commercial, operational, and intelligence authority remain distinct.
- Contracts can survive a change in runtime topology.
- Configuration is versioned and attributable where outcomes can change.

## Future Extensions

- Capability maturity and service-level model.
- Machine-readable capability registry and dependency graph.
- Build-versus-partner decision framework.
- Regional capability overlays.

## Anti-Patterns

- “Hair photo service” instead of a medical media capability.
- A product-specific eligibility engine.
- A shared rules engine with no policy ownership boundaries.
- Direct database integration between capabilities.
- Naming a capability after a current vendor.

## Open Decisions

- Whether communications is an Operations capability or a separate domain.
- Ownership boundary between Medical Media and Source Artifact storage.
- Ownership of experimentation assignment versus analytics evaluation.

