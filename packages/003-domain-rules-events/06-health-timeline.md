# Package 003 Health Timeline

**Status:** Proposed architecture

## Purpose

Define the canonical longitudinal record of a person's HealthOS relationship. The
Timeline is more than an activity log: it is an ordered, provenance-linked index
that makes clinical, commercial, provider, laboratory, media, membership,
fulfillment, communication, protocol, intelligence, and evidence history coherent
without taking ownership away from source domains.

## Timeline Event contract

Each Timeline Event includes Timeline ID, stable event ID and type/version,
subject, occurrence/recording/effective time, source-domain object and Domain Event
references, actor/organization context, summary facts, category, sensitivity,
visibility policy, Evidence references, correction/supersession links, correlation
group, tenant/brand presentation context, and inclusion-policy version.

## Categories

`clinical`, `care-plan`, `treatment`, `provider`, `lab`, `medical-media`,
`document`, `membership`, `commercial`, `order`, `fulfillment`, `communication`,
`protocol`, `intelligence`, `evidence`, and `administrative`. Category never grants
access or determines authority.

## Ingestion

1. Consume a published Domain Event or record an authorized imported fact.
2. Evaluate versioned Timeline inclusion, subject association, and minimization rules.
3. Resolve occurrence time precision and correlation with existing episodes.
4. Append a Timeline Event with source reference and visibility classification.
5. Build audience projections without copying unrestricted source payloads.
6. On correction, append correction/supersession and rebuild affected projections.

## Ordering and uncertainty

The Timeline preserves occurrence time, recorded time, and effective interval.
Events with estimated, date-only, or unknown occurrence time declare precision.
Display order uses a stable policy and MUST NOT fabricate temporal certainty. Domain
aggregate sequences resolve same-source order; cross-domain causal links use
correlation and causation rather than a fictional global sequence.

## Views

| View | Purpose | Boundary |
| --- | --- | --- |
| Patient | Calm explanation of progress, care, orders, and next steps | Hides restricted internal reasoning and operational noise |
| Provider | Clinically relevant history, evidence, decisions, gaps, and provenance | Requires Care Relationship, authority, and purpose |
| Care Team | Assigned tasks, permitted history, communication, and escalation | Does not grant prescribing or full clinical access |
| Operations | Fulfillment, routing, membership, and exception facts | Minimum necessary clinical context only |
| Intelligence | Authorized evidence manifest and longitudinal context | Cannot broaden access or write summaries as source facts |
| Analytics | Governed event semantics and effective-time history | Identity, consent, and secondary-use policies apply |

## Persisted and projected information

| Representation | Treatment |
| --- | --- |
| Persisted Timeline Event | Minimal, historically meaningful, append-only fact with exact source/version, occurrence precision, category, sensitivity, and correction links |
| Dynamically projected state | Current Profile, Membership, plan, Prescription, Order, Shipment, and task summaries resolved from authoritative domains; never copied as mutable Timeline truth |
| Patient-visible | Calm progress, decisions, required next steps, corrections, membership/order/fulfillment milestones, and approved generated assistance under OD-100 policy |
| Provider-visible | Clinically relevant history, Evidence lineage, protocols, decisions, corrections, and authorized operational context |
| Audit-only | Internal security, authority, sensitive rationale, restricted commercial/operational facts, replay metadata, and events whose disclosure would exceed purpose |

The Timeline does not replace the Membership record, Profile snapshot, clinical
chart, or domain audit log. It persists only the minimized longitudinal fact and
source reference; audience projections are rebuilt. Commercial and internal events
are not patient-visible by default.

## Replay and reconstruction

Timeline projections rebuild from Timeline Events and source-domain queries. A
full business-state reconstruction uses the owning Domain Events and records, not
the Timeline alone. Replay supports a declared cutoff (`as known at`) and effective
time (`as applicable at`) and identifies late-arriving or corrected evidence.

## AI summarization

Summaries are Recommendations or Derived Assertions linked to an Evidence Set,
summary policy, model/workflow version, time horizon, audience, and source Timeline
Events. They are visibly generated, expire or regenerate after material correction,
and never replace events. Provider-edited summaries preserve original output and
the Provider's attributable revision.

## Patient experience principles

- Organize around progress and meaning, not raw system traffic.
- Use natural language and progressive disclosure.
- Explain unknown, delayed, corrected, and disputed information calmly.
- Separate “ordered,” “approved,” “shipped,” and “delivered.”
- Never imply purchase equals treatment or generated advice equals Provider judgment.

## State and events

Timeline: `open → restricted | archived`. Timeline Event: `recorded → corrected |
restricted`; visibility projections are rebuildable. Memory publishes
`memory.timeline-event.appended.v1`, `.corrected.v1`, and
`memory.timeline-projection.rebuilt.v1`.

## Permissions and retention

Source-domain permission is re-evaluated for drill-through. Timeline visibility is
purpose- and audience-specific, with break-glass access attributable and reviewed.
Retention and lawful restriction are applied per source and Timeline classification;
“HealthOS does not forget” operates within lawful privacy, correction, retention,
and erasure duties.

## Acceptance criteria

- Every Timeline item resolves to a source or authorized import with provenance.
- Corrections never silently rewrite what was previously shown or decided.
- Patient, Provider, Care Team, Operations, Intelligence, and Analytics views differ
  by policy rather than duplicated records.
- Filtering, replay, visualization, summaries, and longitudinal analysis share
  stable event semantics.
- Timeline unavailability cannot change source-domain truth.

## Future evolution

Episode grouping, external record exchange, personal annotations, caregiver views,
semantic search, longitudinal risk views, and privacy-preserving research timelines.

## Anti-patterns

One mutable activity feed; unrestricted event payload copies; sorting all history by
recorded time; AI summary as medical record; deleting corrected history; using
Timeline projection state as fulfillment or clinical authority.

## Open decisions

- OD-100 — OPEN — DANIEL
