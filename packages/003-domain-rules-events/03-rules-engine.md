# Package 003 Rules Engine

**Status:** Proposed shared capability; policy ownership remains federated

## Purpose

Evaluate versioned, composable, testable, auditable, and explainable business
rules without centralizing domain authority. “Rules Engine” names a capability
and contract, not necessarily one runtime, vendor, language, or service.

## Ownership model

| Concern | Owner |
| --- | --- |
| Clinical eligibility, labs, visits, renewal, provider requirements | Clinical |
| Membership, benefit, pricing, offer, marketplace visibility | Commerce |
| Identity, authority, consent, organization affiliation | Identity |
| Routing, serviceability, fulfillment holds, notifications | Operations |
| Evidence fitness, provenance, timeline inclusion | Memory |
| Recommendation generation, review, release safety | Intelligence |
| Rule execution semantics, conformance, registry mechanics | Platform Architecture / Engineering |

The engine MUST NOT become an ownerless “business logic” domain. Each Rule Set
declares one authoritative owner, purpose, subject type, input contract, output
contract, approvers, effective interval, and safety envelope.

## Canonical objects

| Object | Authoritative owner | Meaning |
| --- | --- | --- |
| Rule Definition | Policy-owning domain | Stable semantic rule and typed inputs, predicate, outcome, reasons, and evidence requirements |
| Rule Version | Policy-owning domain | Immutable approved expression of a Rule Definition |
| Rule Set | Policy-owning domain | Ordered or dependency-based composition of exact Rule Versions for one purpose |
| Rule Set Version | Policy-owning domain | Immutable, effective-dated composition and conflict policy |
| Evaluation Request | Requesting policy domain | Subject, purpose, context, requested time, Evidence Set, and expected contract |
| Rule Result | Policy-owning domain | Per-rule outcome, reasons, inputs used, and evaluation trace |
| Rule Decision | Policy-owning domain | Aggregate outcome with authority, exact Rule Set Version, evidence, validity, and explanation |
| Reason Definition | Policy-owning domain | Stable internal reason mapped to audience-safe explanations |
| Simulation | Policy-owning domain | Non-authoritative evaluation against a declared historical or prospective context |
| Conformance Suite | Policy-owning domain | Approved examples, boundaries, invariants, and regression cases for a Rule Set Version |

“Policy-owning domain” always resolves to exactly one of Identity, Memory,
Clinical, Commerce, Operations, or Intelligence. The shared executor may retain
operational telemetry, but it does not become a second owner of canonical rule records.

## Evaluation contract

1. Resolve the Rule Set Version effective for the requested purpose and time.
2. Verify subject, actor, tenant, jurisdiction, and authority context.
3. Seal or reference the exact Evidence Set.
4. Validate required inputs without converting absence into false.
5. Evaluate deterministic rules and record every result and short-circuit.
6. Compose the authoritative dimension outcome using declared precedence.
7. Produce an audience-safe explanation separately from the internal trace.
8. Persist the Rule Decision and publish its domain-owned event.

Rule decisions use `satisfied`, `not-satisfied`, `unknown`, `not-applicable`, or a
domain-specific enumerated outcome. `Unknown` MUST remain distinct from denial.

## Composition and precedence

- Rule dependencies form an acyclic versioned graph or an explicitly bounded
  iteration; hidden execution order is prohibited.
- A composite eligibility result preserves each domain dimension.
- A negative authoritative clinical rule cannot be overridden by a commercial,
  operational, tenant, marketplace, or intelligence rule.
- Provider judgment is a separate attributable Clinical Decision. It may override
  a recommendation or overridable rule only within a Protocol's explicit bounds.
- Conflicts among rules in one owner domain produce `unknown` or an escalation,
  according to the Rule Set; precedence MUST NOT be inferred from file order.
- Care's lab-free status and Optimize's lab gate are membership/program policy
  inputs only after their precise source-backed mappings are approved.

## Versioning and change control

Rule Versions are immutable after approval. A semantic change creates a successor
with compatibility classification: `clarifying`, `backward-compatible`,
`behavior-changing`, or `safety-critical`. Activation requires owner approval,
Conformance Suite success, effective time, rollback/suspension plan, and impact
analysis. Historical decisions retain the version originally evaluated.

## Explainability and audit

Every Rule Decision MUST identify:

- decision, request, subject, owner, purpose, and evaluated time;
- Rule Set and Rule Versions;
- Evidence Set and material input versions;
- per-rule result, reason, conflict, and short-circuit trace;
- actor or system context, correlation, and causation;
- validity interval and reevaluation triggers;
- internal explanation and permitted audience explanation;
- whether Provider judgment modified an otherwise produced recommendation.

## Rules by use case

| Use case | Minimum dimensions |
| --- | --- |
| Eligibility | Clinical, membership, jurisdiction, serviceability, offer context |
| Profile readiness | required source facts, completeness policy, Profile projection time, missing-data reasons |
| Age / BMI | Measurement time, evidence source, units, boundary, protocol version |
| Labs | panel, biomarkers, collection time, freshness, validation, exception policy |
| Provider / visit | credential, scope, jurisdiction, modality, synchronous requirement |
| Medical media | type, views, quality, recency, reviewer, retry/escalation |
| Renewal | treatment state, monitoring evidence, cadence, Provider review |
| Pricing | Price Version, currency, audience, benefit, tax/fee policy, effective time |
| Routing | authorized formulation, pharmacy capability, jurisdiction, serviceability, fallback |
| Marketplace | Product publication, offer eligibility, content/compliance approval, explanation |
| Intelligence | use case, evidence allowance, model release, confidence use, required review |

## Protocol, rules, recommendation, and Provider decision

The clinical sequence is deliberately non-collapsible:

1. An effective Protocol Version defines guidance, required Evidence, permitted
   options, and the bounds of professional discretion.
2. Exact Rule Set Versions evaluate authoritative facts and produce explainable
   Rule Results or a candidate status; the Rules Engine owns neither facts nor policy.
3. Intelligence or deterministic workflow may create a system Recommendation that
   preserves the Rule Decision and Evidence Set.
4. An authorized Provider reviews the evidence and Recommendation through Provider Review.
5. The Provider creates the final patient-specific Eligibility Decision and Care Plan.
6. An override preserves the system result, Protocol and Rule Set Versions,
   Recommendation, reviewed Evidence Set, Provider decision, authority, and required rationale.
7. Care Plan eligibility may make a Medication available for later selection; it
   does not itself create a Prescription.
8. After the patient selects the Medication and requests purchase/payment
   authorization, Prescription Review may approve or decline the Prescription.
9. Only an approved Prescription and satisfied commercial/operational gates make
   the Order releasable to Pharmacy.

Provider override creates a new decision; it MUST NOT edit the Rule Result,
Recommendation, Protocol, or underlying Evidence. Commercial configuration cannot
change clinical rules or the membership prerequisite.

## Locked membership rules

- One Membership supports a primary Profile and up to two invited, clinically
  independent guest Profiles that share the Membership dependency.
- Care and Optimize are parallel program enrollments, not Membership products.
- Care has no program-level lab gate. Optimize Treatment Plans are lab-gated. Labs
  do not universally gate marketplace visibility or every Medication.
- Monthly and annual are configurable Billing Options for the same Membership product.
- During dunning, existing Treatment continues; new Prescriptions and refills are
  held. Resumption requires clinically appropriate reevaluation, not automatic release.
- Cancellation is blocked while any attached Profile has non-terminal Treatment.
  A primary override-cancel is attributable and follows the approved policy for
  clearing attached Treatment Plans.

## State machines and events

Rule Definition: `draft → review → approved → active → suspended | retired | superseded`.
Evaluation: `requested → validating → evaluating → complete | incomplete | failed | expired`.

Events include `rules.rule-set.activated.v1`, domain-owned `*.rule-decision.made.v1`,
`rules.evaluation.failed.v1`, and `rules.conformance.failed.v1`. The generic events
describe execution; the policy-owning domain publishes the business outcome.

## Permissions and safety

- Authors, reviewers, approvers, activators, and emergency suspenders are distinct roles.
- Clinical Rule Sets require Clinical approval; pricing requires Commercial approval;
  jurisdiction and retention constraints require Compliance approval.
- Rule authors cannot grant their own actor permissions through a rule.
- Secrets, unrestricted protected data, and executable arbitrary code are prohibited inputs.
- Simulations and previews MUST be marked non-authoritative and cannot trigger effects.

## Acceptance criteria

- A decision can be reproduced from preserved versions and evidence.
- Boundary, missing-data, conflict, and adverse-path cases exist in conformance tests.
- Rule changes can be compared against historical scenarios before activation.
- Every result has owner-specific reasons and safe explanations.
- No shared engine contract collapses clinical and commercial authority.

## Future evolution

Machine-readable rule interchange, visual authoring, differential simulation,
formal safety invariants, policy-as-code adapters, and regional overlays may be
added without changing ownership or decision contracts.

## Anti-patterns

- One global `isEligible` rule set.
- Business users editing clinical thresholds through generic admin settings.
- Current rules reinterpreting historical decisions.
- AI-generated predicates activated without owner approval and conformance.
- Rules directly writing Orders, Prescriptions, or Timeline Events.

## Open decisions

- OD-103 — OPEN — ENGINEERING
- OD-012 — OPEN — ENGINEERING
