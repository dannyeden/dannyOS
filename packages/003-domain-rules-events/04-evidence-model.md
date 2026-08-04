# Package 003 Evidence Model

**Status:** Proposed architecture

## Purpose

Make material clinical and automated outcomes traceable to immutable, attributable
evidence while preserving the difference among observation, assertion, inference,
recommendation, provider judgment, and decision.

## Evidence taxonomy

| Evidence kind | Example | Authority note |
| --- | --- | --- |
| Patient assertion | Questionnaire answer, attestation, reported history | Attributed claim; verification state is explicit |
| Measured observation | Lab result, vital sign, device reading | Source, method, units, specimen/device, and time required |
| Medical media | Photo, video, scan, audio | Capture, transformation, and quality lineage required |
| Source document | Lab report, medication label, referral | Original retained; extraction is separate evidence |
| Provider evidence | Note, examination finding, judgment | Actor authority, organization, purpose, and encounter required |
| Treatment history | Prior Treatment, Prescription, Fill, adherence assertion | References canonical records and effective context |
| Communication | Message or call record | Participants, channel, time, and completeness limits required |
| Generated evidence | Derived Assertion, Recommendation | Never source fact; model/workflow and Evidence Set required |
| Commercial/operational fact | Membership, Order, Shipment, routing fact | May gate workflows but does not become clinical judgment |

## Canonical envelope

Every Evidence record contains stable ID, subject, kind, claim or observation,
source and author, occurred/recorded/effective times, provenance, verification and
dispute state, sensitivity, intended and prohibited uses, jurisdiction/tenant
context, source-artifact reference, correction lineage, and cryptographic integrity
metadata where required. Structured extraction references location within the
source and never replaces the original.

An Evidence reference used in a decision MUST identify the source object and
source version, observation or collection date and precision, validation status,
actor, Evidence version, and the exact decision/Evidence Set use. Later source
changes do not reinterpret a past decision; they create corrected Evidence and,
when material, a new evaluation.

## Evidence Set and decision linkage

An Evidence Set is an immutable manifest, not copied evidence. It records inclusion
and exclusion policy, assembler, purpose, conflicts, missing requirements, freshness
at assembly, and the exact Evidence versions. Eligibility Decisions, Provider
Reviews, Care Plans, Prescriptions, Rule Decisions, and Recommendations MUST link
the Evidence Set that materially informed them.

## Fitness for use

Evidence is not universally “valid.” Fitness is evaluated for a specific purpose:

- identity and subject match;
- source authority and verification;
- occurrence time, freshness, and effective interval;
- completeness, quality, units, and reference context;
- conflicts, corrections, and supersession;
- consent, purpose, jurisdiction, and allowed audience;
- protocol or Rule Set requirements.

An unfit item remains historically real evidence but is excluded or escalated for
the stated purpose. Confidence MUST NOT be used as a substitute for authority.

## Immutability, correction, and lawful controls

- Recorded content is append-preserving.
- Correction creates linked replacement Evidence and records reason and actor.
- Dispute records a challenge without silently changing verification.
- Supersession changes current applicability, not historical existence.
- Redaction or erasure obligations use restricted content, tombstones, or lawful
  deletion workflows while preserving the minimum permitted audit lineage.
- Derived evidence is invalidated or reevaluated when a material source is corrected.

## Provider judgment and AI

Provider judgment may be recorded as clinical reasoning Evidence, decision
rationale, or both; the canonical boundary and minimum duplication remain OPEN —
CLINICAL under OD-105. In every option, the resulting authorization is a separate
Clinical Decision and neither record silently replaces the other. Recommendations
preserve their source Evidence Set, model/workflow release, material instruction or
Prompt Version, timestamp, uncertainty where appropriate, reviewer disposition,
and resulting decision reference. AI output cannot verify its own evidence or
silently promote a derived assertion. The retention policy preserves enough
generation provenance for audit without promising indefinite retention of raw
prompts, hidden model internals, or unnecessary sensitive context.

## Appeals, audit, and improvement

An appeal view reconstructs the decision, Evidence Set, rules/protocol, reasons,
authority, later corrections, and permitted explanation. Protocol improvement and
research use require separately approved purpose, minimization, cohort governance,
and reproducible source lineage; ordinary care evidence is not training data by
default.

## Events

`memory.evidence.recorded.v1`, `memory.evidence.verified.v1`,
`memory.evidence.disputed.v1`, `memory.evidence.superseded.v1`,
`memory.evidence.restricted.v1`, and `memory.evidence-set.sealed.v1`.

## Permissions

Memory controls evidence integrity and authorized access. Clinical controls fitness
for clinical use. Compliance controls retention, disclosure, and secondary-use
policy. Intelligence cannot broaden source permissions. A Timeline projection does
not grant access to underlying Evidence.

## Acceptance criteria

- Every material clinical decision identifies its Evidence Set.
- Original content and correction lineage are reconstructable where law permits.
- Evidence conflicts and missing required evidence remain visible.
- An actor can distinguish source facts, assertions, derived evidence,
  Recommendations, Provider judgment, and decisions.
- Access, retention, and secondary use are purpose-bound and auditable.

## Future evolution

External clinical exchange, digital signatures, device trust, provenance graphs,
privacy-preserving research, evidence-quality scoring, and provider confidence
support may extend the model without changing its authority distinctions.

## Anti-patterns

Mutable “latest answer” fields; copying evidence into every decision; AI summaries
as source records; universal confidence scores; deleting disputed evidence; using
analytics eligibility as consent for research.

## Open decisions

- OD-022 — OPEN — CLINICAL
- OD-051 — OPEN — COMPLIANCE
- OD-052 — OPEN — COMPLIANCE
- OD-056 — OPEN — COMPLIANCE
- OD-105 — OPEN — CLINICAL
