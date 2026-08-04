# Package 003 Event Architecture and Catalog

**Status:** Proposed architecture

## Purpose

Define immutable business facts used for cross-domain communication and durable
state-transition history. This is a logical contract, not a mandate for a broker,
service topology, or event-sourced implementation.

## Domain Event envelope

Every published Domain Event includes:

- globally unique event ID, canonical type, schema version, and producer domain;
- aggregate type and stable ID, tenant/organization scope where applicable;
- `occurredAt`, `recordedAt`, actor/system context, correlation and causation IDs;
- command or transition idempotency key and aggregate sequence where promised;
- sensitivity and disclosure classification;
- payload with stable canonical references rather than unnecessary sensitive data;
- schema reference, compatibility declaration, and trace/provenance reference.

Names follow `<domain>.<subject>.<past-fact>.vN`. Commands and requests are not
events. A failure event states that a meaningful failure occurred; it is not a log line.

## Delivery semantics

- Producers guarantee durable publication after the owning state transition.
- Consumers MUST be idempotent by event ID and business effect key.
- At-least-once delivery is assumed unless a contract explicitly guarantees more.
- Ordering is guaranteed only per declared aggregate sequence; global order is prohibited.
- Consumers detect gaps when sequence is required and reconcile through an owned query.
- Replays preserve original event ID and occurrence time and add replay context out of band.
- Corrections and compensations publish new facts; prior events are never edited.
- Consumer failure MUST NOT roll back a completed producer-domain fact.
- Contracts include the IDs, versions, and decision-ready reason facts consumers
  need to react; no consumer is required to synchronously read producer-private internals.

## Versioning

Backward-compatible additive payload changes stay within the schema version only
when consumers can safely ignore them. Changed meaning, required fields, identifiers,
or lifecycle semantics require a new event version. Producers support an explicit
transition window; consumers declare supported versions. Historical replay uses the
original schema and a registered upcaster/projection adapter when necessary.

## Retention and replay classes

| Class | Examples | Expectation |
| --- | --- | --- |
| Constitutional audit | Clinical decisions, consent, authority, prescriptions | Retain per governing policy; replay tightly controlled |
| Longitudinal | Treatment, labs, membership, fulfillment outcomes | Preserve source facts and Timeline projection eligibility |
| Operational | work assignment, delivery attempts, retries | Retain for audit/service policy; may have shorter payload retention |
| Ephemeral signal | cache invalidation, non-material progress | Not canonical; consumers reconcile from owner |

Retention policy is not embedded as a hard-coded period. Compliance owns policy;
the event contract declares classification and minimum reconstruction requirements.

## Event catalog

The payload column names minimum business references, not a physical schema.

| Event | Producer | Primary consumers | Minimum payload | Ordering / replay |
| --- | --- | --- | --- | --- |
| `identity.person.resolved.v1` | Identity | Memory, Clinical, Commerce | Person, resolution decision, prior aliases | Person sequence; rebuild references |
| `identity.role-assignment.changed.v1` | Identity | Clinical, Operations, Audit | Actor, organization, role, validity | Assignment sequence; reevaluate authority |
| `identity.consent.changed.v1` | Identity | Memory, Operations, Intelligence | Person, purpose, scope, validity | Consent sequence; revoke future use |
| `commerce.membership.activated.v1` | Commerce | Clinical, Operations, Memory | Membership, Person, plan/version, effective time | Membership sequence; rebuild gate |
| `commerce.payment.failed.v1` | Commerce | Membership workflow, Memory | Payment Intent, Membership reference, failure class, occurred time | Payment sequence; replay records failure but does not independently enter dunning |
| `commerce.billing-arrangement.entered-dunning.v1` | Commerce | Clinical, Operations, Memory | Billing Arrangement, Membership, attached Profile references, effective time, triggering payment facts | Billing Arrangement sequence; hold new Prescription/refill workflows without changing Membership or existing Treatment |
| `commerce.membership.expired.v1` | Commerce | Clinical, Operations, Memory | Membership, expiration time, reason | Membership sequence; apply approved policy |
| `clinical.questionnaire.completed.v1` | Clinical | Memory, Rules, Intelligence | Instance, Questionnaire Version, Evidence Set | Instance terminal fact; safe replay |
| `clinical.lab-result.validated.v1` | Clinical | Memory, Rules, Provider workflow | Result, Biomarker, evidence, validator | Result sequence; trigger reevaluation |
| `clinical.labs.validated.v1` | Clinical | Evaluation, Memory | Lab Panel, result set, validity | Panel sequence; trigger protocol gate |
| `clinical.eligibility.determined.v1` | Clinical | Commerce, Operations, Memory | Provider-owned decision, Evidence Set, Protocol/Rule Set Versions, outcome, reasons, validity | Evaluation sequence; replace by new decision only |
| `clinical.provider-review.requested.v1` | Clinical | Operations | Review, patient, required authority, due policy | Review sequence; assign work idempotently |
| `clinical.provider-review.completed.v1` | Clinical | Operations, Memory, Intelligence | Review, disposition, Provider context, decision | Review sequence; preserve original suggestion |
| `clinical.care-plan.approved.v1` | Clinical | Memory, Commerce, Operations | Care Plan Version, patient, approver, eligible Medication references, effective time | Plan sequence; enables later selection but creates no Prescription |
| `clinical.treatment.started.v1` | Clinical | Memory, Operations, Analytics | Treatment, plan, start time | Treatment sequence; never inferred from purchase |
| `clinical.prescription.approved.v1` | Clinical | Commerce, Operations, Memory | Prescription, purchase-request reference, patient, formulation constraints, Evidence/Protocol Versions, Provider | Prescription sequence; makes eligible Order releasable but does not prove capture or fulfillment |
| `clinical.provider-override.recorded.v1` | Clinical | Memory, Intelligence, Safety | system result, final Provider decision, rationale where required, Evidence Set, Protocol/Rule/Recommendation Versions, authority | Decision sequence; replay for audit without rewriting source result |
| `commerce.offer.published.v1` | Commerce | Marketplace, Analytics | Offer Version, audience policy, interval | Offer sequence; historical presentation replay |
| `commerce.order.placed.v1` | Commerce | Operations, Memory | Order, buyer, offer/price snapshot, authorization state | Order sequence; records order intent and never alone creates medication fulfillment |
| `commerce.medication-purchase.requested.v1` | Commerce | Clinical, Operations, Memory | purchase request, Order line, patient, Medication/Product, Offer/Price Versions, payment-authorization state | Order-line sequence; request Prescription Review, not capture, Prescription approval, or Treatment start |
| `operations.pharmacy.routed.v1` | Operations | Pharmacy workflow, Memory | routing decision, Fill, Pharmacy, configuration | Fill sequence; preserve alternatives/reasons |
| `operations.fulfillment-request.released-to-pharmacy.v1` | Operations | Pharmacy, Memory, Commerce | Fulfillment Request, Order, Prescription, routing decision, exact pharmacy configuration | Fulfillment sequence; external release effect occurs once and replay suppresses resend |
| `operations.fill.held.v1` | Operations | Commerce, Care Team, Memory | Fill, hold reason, source fact | Fill sequence; reversible by release fact |
| `operations.fill.released.v1` | Operations | Pharmacy workflow, Memory | Fill, resolved hold, authorizing facts | Fill sequence; idempotent release |
| `operations.shipment.dispatched.v1` | Operations | Member experience, Memory | Shipment, Fill, carrier reference, time | Shipment sequence; replay tracking projection |
| `operations.shipment.delivered.v1` | Operations | Commerce, Clinical, Memory | Shipment, delivery evidence, time | Shipment sequence; may trigger follow-up |
| `memory.medical-media.recorded.v1` | Memory | Clinical, Intelligence | Medical Media ID, subject, capture-requirement version, provenance and consent references | Media sequence; payload never embeds media; resolve authorized view |
| `memory.document.verified.v1` | Memory | Clinical, Identity, Rules | Document, type, verifier, Evidence refs | Document sequence; trigger gated evaluation |
| `memory.timeline-event.appended.v1` | Memory | Timeline projections, Analytics | Timeline Event, subject, source reference | Timeline sequence; projection-only fact |
| `intelligence.recommendation.generated.v1` | Intelligence | Clinical or other requesting domain, Memory | Recommendation, Evidence Set, Model/Workflow and material Prompt Versions, uncertainty, validity | Recommendation sequence; never auto-decide |
| `intelligence.recommendation.disposition-recorded.v1` | Intelligence | Memory, evaluation | Recommendation, reviewer disposition, consuming-domain decision reference | Recommendation sequence; records review after the consuming domain owns the decision |
| `intelligence.safety-signal.detected.v1` | Intelligence | Safety, Compliance, owner domain | use case, release, signal class, evidence refs | Signal sequence; replay restricted |

## Timeline relationship

A Domain Event MAY qualify for zero, one, or multiple Timeline Events. Memory applies
a versioned inclusion and visibility policy and stores a source reference. The
Timeline Event does not republish unrestricted payloads or become the source of the
domain fact. Pure transport retries and cache signals never appear in the Timeline.

## Permissions and privacy

Event availability is not authorization. Consumers are registered by purpose,
contract version, tenant scope, and minimum data. Sensitive details are resolved
through authorized owner queries. Replays require approval, isolation, audit, and
side-effect controls; notification, payment, and pharmacy consumers default to
replay-safe dry processing unless explicitly authorized.

## Acceptance criteria

- Every meaningful transition emits one owned past-tense fact.
- Duplicate delivery cannot create duplicate Orders, Fills, decisions, or messages.
- Consumers tolerate compatible versions and can detect required ordering gaps.
- Replay cannot accidentally repeat external side effects.
- Event retention and Timeline retention remain separate policies.

## Anti-patterns

Generic `statusChanged`; events as commands; full clinical records in every payload;
global ordering; consumer writes to producer state; deleting incorrect events;
making a broker the longitudinal record.

## Open decisions

- OD-098 — OPEN — ENGINEERING
