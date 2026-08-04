# Package 003 Semantic Architecture Review

**Review date:** 2026-08-03

**Status:** Complete pre-commit review

**Scope:** Package 003, Package 003 governance edits, and semantic reconciliation
against Foundation Draft 0.1 and Package 002. Package 002 files were read-only.

## 1. Executive conclusion

**APPROVE WITH CORRECTIONS**

The package is architecturally coherent after the corrections recorded below. No
critical finding remains. The six bounded contexts have exclusive ownership,
shared rule execution preserves federated policy authority, Evidence and Timeline
semantics preserve historical truth, and the clinical purchase workflow no longer
collapses care-plan eligibility, patient selection, payment authorization,
Prescription approval, and pharmacy release.

The remaining open decisions are legitimate governance, implementation-contract,
experience, compliance, or Clinical policy choices. They do not justify inventing
defaults in this package.

## 2. Findings

| Severity | Affected file | Concept | Problem | Consequence | Correction | Open decision |
| --- | --- | --- | --- | --- | --- | --- |
| MATERIAL | `01`, `02`, `07` | Profile | Profile was described as an Identity preferences shell, conflicting with the locked current health-state snapshot meaning. | Identity could become a second owner of clinical state and Profile could drift from Timeline/source domains. | Assigned Profile exclusively to Memory as a rebuildable `as-of` projection with source-version references; Identity remains owner of Person/account/preferences facts. | No |
| MATERIAL | `02`, `03`, `05`, `07` | Protocol-to-Prescription workflow | Rule output, Eligibility Decision, purchase, Prescription, Order, and fulfillment sequence allowed premature Prescription/fulfillment implications. | A purchase event could be read as clinical approval or Treatment initiation. | Made Rule Decision a candidate; Provider owns final patient-specific decision/Care Plan; patient selection and payment authorization request Prescription Review; pharmacy release follows approved Prescription and remaining gates. | No |
| MATERIAL | `02`, `03`, `05`, `07`, AD-005 | Membership and billing dunning | Draft covered Fill freeze but not the locked no-new-Prescription/refill rule, clinically reviewed resumption, attached Profiles, cancellation block, or Billing Arrangement ownership of dunning. | Dunning could become a Membership state, automatically resume care, or permit new clinical authorization inconsistent with the membership model. | Preserved the separate Billing Arrangement lifecycle; added primary plus two guest Profiles, Care/Optimize parallel enrollment, billing-option distinction, cancellation guard, attributable override-cancel, no new Prescriptions/refills during dunning, and clinical reevaluation before resumption. | Existing OD-060/061 govern remaining transition detail |
| MATERIAL | `canon/08`, `canon/12`, `canon/13` | Stale Foundation dunning wording | Draft chapters still describe a Fill-only freeze, while the locked model now prohibits new Prescriptions and refills and requires clinical reevaluation. | Readers using those chapters alone could implement an incomplete gate. | AD-005 was clarified in the in-scope Decision Log, which outranks Foundation Draft chapters. Prior chapters were not rewritten under this review's constraints and require a later additive Canon reconciliation. | No; policy is resolved |
| MATERIAL | `05` | Medication purchase event | `MedicationPurchased` implied capture and completed purchase despite payment authorization pending Clinical review. | Analytics, Timeline, or Operations could infer Prescription approval, capture, or Treatment start. | Renamed to `commerce.medication-purchase.requested.v1`; added explicit payment-failure and release-to-pharmacy facts. Downstream consumers must migrate event names before contract publication. | No |
| MATERIAL | `05`, `02` | Recommendation disposition | Intelligence-produced `RecommendationAccepted` blurred the consuming domain's decision authority. | An AI review status could masquerade as a clinical decision. | Replaced with `intelligence.recommendation.disposition-recorded.v1`; the consuming domain owns the resulting decision and Intelligence only records disposition/reference. | No |
| MATERIAL | `02` | Medication and pharmacy identities | The Package 003 object catalog did not explicitly restate inherited Medication/Formulation/SKU ownership. | Reviewers could infer Product, patient-facing medication, clinical Medication, or pharmacy configuration were interchangeable. | Added a single-owner inherited-boundary table and preserved Package 002 configuration/history invariants. | No |
| MATERIAL | `04` | Provider judgment | Draft treated Provider judgment as Evidence without resolving whether it is Evidence, decision rationale, or both. | Implementations could duplicate or omit clinically material reasoning. | Left the boundary explicit and opened OD-105; authorization remains a separate Clinical Decision in all options. | Yes — OD-105 |
| MINOR | `04` | AI provenance | “Generation context” could be read as indefinite retention of raw prompts/model internals. | Excess sensitive data might be retained without audit value. | Require material prompt/instruction versions and sufficient provenance, not indefinite raw prompt or hidden-internal retention. | Existing retention decisions apply |
| MINOR | `06` | Timeline persistence | Persisted Timeline facts and dynamic audience projections were implied but not enumerated. | Timeline could become a copied current-state store or expose internal events. | Defined persisted, dynamic, patient, Provider, and audit-only representations and correction behavior. | OD-100 governs patient default |
| MINOR | `08` | Multi-tenant MVP | Eden-first proportionality did not explicitly exclude several future platform requirements. | Teams could overbuild tenant administration or permit tenant clinical publishing. | Explicitly deferred tenant admin, internationalization, complex residency, cross-tenant sharing, and tenant-authored Protocols; Eden governance remains initial authority. | OD-101 |
| MINOR | `09` | Design direction | The emotional direction lacked warm-white/low-noise, memory-over-sessions, and urgent-clinical prominence tests. | “Calm” could become decorative minimalism that hides safety information. | Added Apple-like restraint, privacy/trust, warm-white spaciousness, memory over sessions, and clinical clarity over minimalism. | OD-102 |
| CLARIFICATION | `OPEN-DECISIONS`, `04`, `10` | Decision quality | OD-097, OD-099, and OD-104 duplicated existing retention, evidence-freshness, ratifier, quorum, and release decisions. | Parallel decisions could produce conflicting authority. | Removed duplicates and referenced OD-022, OD-051, OD-052, OD-056, OD-002, OD-006, and OD-057. | No new decision |
| CLARIFICATION | AD-008–AD-012 | Accepted-decision traceability | Decisions stated consequences but not affected chapters or superseded assumptions. | Reviewers could misread recommendations as implementation mandates. | Added affected-document links and explicit supersession statements. | No |

### Critical findings

None after correction.

## 3. Contradiction review against Packages 001 and 002

| Established concept | Package 003 result | Status |
| --- | --- | --- |
| Six top-level domains with one owner per object | Formalized as bounded contexts; runtime topology remains open | Aligned |
| Person, Member, Patient, Account, and Membership are distinct | Person is Identity; Member is a contextual relationship term, not a duplicate aggregate; Membership is Commerce; Patient is a clinical context | Aligned |
| Timeline Event differs from Domain Event | Memory-owned longitudinal record remains separate from integration contract | Aligned |
| Product, Formulation, Pharmacy SKU, Prescription, and Fill are distinct | Explicit inherited owner table and lifecycle separation added | Aligned |
| Patient-facing medication survives pharmacy changes | Patient-facing presentation is Commerce; clinical Medication/Formulation are Clinical; pharmacy configuration is Operations | Aligned |
| Offer cannot override clinical rules | Rule precedence and clinical sequence make this explicit | Aligned |
| Provider may override recommendations; Care Team cannot prescribe | Provider decision is attributable and distinct from Recommendation; Care Team permissions are bounded | Aligned |
| Treatment history is not overwritten | Treatment, Evidence, Timeline, Prescription, Order, and Fill corrections append lineage | Aligned |
| Membership prerequisite and dunning coordination | Billing Arrangement retains dunning; effects are strengthened to locked no-new-Prescription/refill and reevaluation behavior | Corrected clarification to AD-005 |
| Care lab-free; Optimize lab-gated | Program-level distinction is explicit; labs do not universally gate marketplace or all Medication | Aligned |
| Multiple billing options for one Membership | Monthly/annual remain Billing Options, not separate Membership products | Aligned |
| Package 002 formulary is pharmacy-available, not automatically clinically/marketplace ready | Product/pharmacy boundary and routing remain independent | Aligned |
| Source rows and historical pharmacy configurations remain lossless | Package 003 references immutable configuration identity; Package 002 is unchanged | Aligned |

## 4. Object-ownership audit

| Object | Authoritative owner | Referenced/event consumers | Duplication result |
| --- | --- | --- | --- |
| Person | Identity | All domains by stable ID and authority facts | No duplicate; Member/Patient are contexts |
| Member | Not a separate aggregate | Commerce, Clinical, Memory | Canonical relationship term over Person, not Membership identity |
| Membership | Commerce | Clinical, Operations, Memory | Distinct from Person/Profile/program enrollment |
| Profile | Memory | Clinical, Commerce, Intelligence, patient views | Current snapshot; does not duplicate Timeline or chart |
| Health Timeline / Timeline Event | Memory | Patient, Provider, Operations, Intelligence, Analytics | Projection/index only; no source-domain mutation |
| Evidence / Evidence Set | Memory | Clinical, Rules, Intelligence, Audit | Immutable inputs, not decisions or summaries |
| Document / Medical Media | Memory | Identity/Clinical/Intelligence under purpose | Specialized Evidence sources; no dual owner |
| Recommendation | Intelligence | Clinical and other requesting domains, Memory | Non-authoritative; disposition links final decision |
| Eligibility Decision | Clinical for clinical dimension | Commerce, Operations, Memory | Distinct from Rule candidate and Prescription |
| Protocol | Clinical | Rules, Product mappings, Provider Review | Guidance/evidence envelope; not Rule execution or Product |
| Rule / Rule Set | Owning policy domain | Shared execution, consuming workflows | Each rule has one of six owners; engine mechanics do not own policy |
| Provider Review | Clinical | Operations assignment, Memory, Intelligence | Workflow for accountable Provider judgment |
| Care Plan | Clinical | Memory, Commerce, Operations | Broad multi-goal/multi-medication decisions |
| Treatment Plan | Clinical | Treatment, Prescription, Operations | Dosing/monitoring/duration/follow-up structure |
| Treatment | Clinical | Memory, Operations, Analytics | Patient-specific therapy instance |
| Prescription | Clinical | Commerce, Operations, Memory | Clinician authorization to dispense |
| Product | Commerce | Clinical mappings, Marketplace, Operations | Not Medication, Offer, Price, Protocol, or SKU |
| Patient-facing medication presentation | Commerce | Marketplace, Clinical, Operations | Stable display identity independent from pharmacy config |
| Medication / Formulation / Additive / Strength | Clinical | Commerce and Operations mappings, Memory | Clinical therapy/composition hierarchy |
| Package / Pharmacy SKU / Pharmacy Source | Operations | Clinical constraints, Commerce references, Memory | Fulfillment identity; never Product or Price |
| Offer / Price | Commerce | Marketplace, Order, Analytics | Versioned terms/money; distinct from Product and pharmacy economics |
| Order | Commerce | Operations, Memory | Financial/fulfillment request; not Prescription or Treatment |
| Pharmacy / Shipment / Fill | Operations | Commerce, Clinical, Memory | Partner and fulfillment lifecycles; no clinical authority |

No reviewed object has two authoritative owners or no owner. Experiment assignment
remains Commerce while Analytics owns Metric Definitions/evaluation, which are
separate objects rather than shared ownership of Experiment.

## 5. Event-ownership audit

All original 30 events were reviewed. Two missing completed facts were added
(`commerce.payment.failed.v1` and
`operations.fulfillment-request.released-to-pharmacy.v1`), producing a corrected
32-event catalog.

| Events | Producer | Audit result |
| --- | --- | --- |
| `identity.person.resolved.v1`; `identity.role-assignment.changed.v1`; `identity.consent.changed.v1` | Identity | Past-tense owner facts; stable IDs/validity; Person/assignment/consent ordering only |
| `commerce.membership.activated.v1`; `commerce.payment.failed.v1`; `commerce.billing-arrangement.entered-dunning.v1`; `commerce.membership.expired.v1` | Commerce | Payment failure and Billing Arrangement dunning remain separate from Membership lifecycle; per-aggregate ordering; replay does not re-charge or auto-transition |
| `clinical.questionnaire.completed.v1`; `clinical.lab-result.validated.v1`; `clinical.labs.validated.v1` | Clinical | Exact definition/result/evidence references; no answers, documents, or PHI copied unnecessarily |
| `clinical.eligibility.determined.v1`; `clinical.provider-review.requested.v1`; `clinical.provider-review.completed.v1` | Clinical | Candidate rules separated from Provider-owned determination; review assignment is idempotent |
| `clinical.care-plan.approved.v1`; `clinical.treatment.started.v1`; `clinical.prescription.approved.v1`; `clinical.provider-override.recorded.v1` | Clinical | Each is a completed clinical fact; purchase is not Treatment; override preserves system/final results and versions |
| `commerce.offer.published.v1`; `commerce.order.placed.v1`; `commerce.medication-purchase.requested.v1` | Commerce | Offer/order versions retained; purchase request states authorization intent, not capture or Prescription approval |
| `operations.pharmacy.routed.v1`; `operations.fulfillment-request.released-to-pharmacy.v1`; `operations.fill.held.v1`; `operations.fill.released.v1` | Operations | Routing/release/hold facts are distinct; external release effect is once-only and replay-suppressed |
| `operations.shipment.dispatched.v1`; `operations.shipment.delivered.v1` | Operations | Shipment sequence only; delivery evidence referenced rather than copied |
| `memory.medical-media.recorded.v1`; `memory.document.verified.v1`; `memory.timeline-event.appended.v1` | Memory | Media payload contains no media; source/version/consent references preserved; Timeline event remains projection fact |
| `intelligence.recommendation.generated.v1`; `intelligence.recommendation.disposition-recorded.v1`; `intelligence.safety-signal.detected.v1` | Intelligence | Generation and disposition are not clinical decisions; exact release/evidence/instruction references retained |

The envelope gives all events event-ID idempotency, correlation/causation,
sensitivity, versioning, and replay context. The catalog specifies per-aggregate
ordering only where meaningful. Consumers receive decision-ready identifiers,
versions, and reason classes and are not required to synchronously query private
producer internals. Payloads reference PHI/media rather than duplicating it.

### Event rename consequences

- Draft consumers of `commerce.membership.dunning-started.v1` must align to the
  existing canonical `commerce.billing-arrangement.entered-dunning.v1` before publication.
- `commerce.medication.purchased.v1` is withdrawn before publication and replaced
  by `commerce.medication-purchase.requested.v1`; capture analytics must use a
  separate Payment fact.
- `intelligence.recommendation.accepted.v1` is withdrawn before publication and
  replaced by `intelligence.recommendation.disposition-recorded.v1`; consuming-domain
  decision events remain authoritative.
- The two added events require consumers to preserve payment-failure/dunning and
  Prescription-approval/pharmacy-release boundaries.

No released external contract exists, so these are draft compatibility corrections,
not production migrations.

## 6. Rules Engine boundary audit

The Rules Engine is correctly a shared execution capability, not a seventh policy
domain. Rules reference authoritative Membership, Profile, age/BMI Evidence, Labs,
jurisdiction, visits, media, renewal, Price/Offer, marketplace, pharmacy-routing,
and Experiment facts. The owning domain approves each Rule Set. Every evaluation
records exact versions, Evidence Set, trace, reason, validity, actor, and explanation.

Provider overrides create a new Clinical Decision and preserve the Rule Result,
Protocol, Recommendation, evidence, authority, and rationale. Commercial or tenant
configuration cannot edit clinical Protocols/Rules or remove the Membership prerequisite.

## 7. Evidence, Timeline, membership, media, and Intelligence audit

- **Evidence:** Append-preserving, source/version/date/validation/actor aware, and
  linked through immutable Evidence Sets to exact decisions. Corrections do not
  reinterpret past decisions. OD-105 keeps Provider judgment's dual
  evidence/rationale boundary explicit.
- **Timeline:** Persists minimized historical events and exact source references;
  current Profile, Membership, chart, Order, and Shipment state are dynamic source-
  domain projections. Patient, Provider, and audit views are distinct, and
  corrections append rather than delete.
- **Membership:** One Membership supports the primary plus up to two clinically
  independent invited guest Profiles. Care and Optimize are parallel enrollments;
  only Optimize Treatment Plans have the program-level lab gate. Dunning preserves
  existing Treatment, holds new Prescriptions/refills, and requires clinical
  reevaluation before resumption. Cancellation guards cover all attached Profiles.
- **Care Team and Provider:** Care Team preparation, lab validation, questionnaire,
  routing, permitted pause, and escalation actions remain non-prescribing. Providers
  own final clinical eligibility, patient-specific plan changes, overrides,
  additional evidence requirements, and Prescription approval/decline.
- **Medical Media:** Reusable Memory capability with separate consent, restricted
  access, immutable original metadata, separate annotations/AI derivatives, and
  Protocol-configurable required/recommended/optional status. Baseline full-body
  media is strongly recommended for Profile creation and weight/body-composition
  progress, not universally required or available for marketing reuse.
- **Intelligence:** May summarize, recommend, prioritize, draft, prepare, detect
  discrepancy, suggest, and explain authorized Evidence. It may not silently
  prescribe, validate unverified Labs, alter clinical decisions/Protocols, publish
  commercial changes, change routing, or make untraceable eligibility decisions.
  Provenance is sufficient for audit without requiring indefinite raw prompt or
  hidden model-internal retention.

## 8. Open-decision quality

Three Package 003 decisions were removed as duplicates: OD-097 merged into existing
OD-051/052/056, OD-099 into OD-022, and OD-104 into OD-002/006/057.

| ID / owner | Why and affected scope | Realistic options | Recommended default | Delay consequence | Block class |
| --- | --- | --- | --- | --- | --- |
| OD-096 — DANIEL | Named stewards and arbitration affect all six contexts and every cross-domain contract | Named individual stewards; role-based stewards; Architecture owner interim | Platform Architecture arbitrates with affected domain authority until named stewards are approved | Slows ratification and conflict escalation | Ratification; not MVP launch |
| OD-098 — ENGINEERING | Delivery/order/replay standard affects all Domain Events and side-effect consumers | Broker-specific guarantee; contract-level at-least-once; transactional log adapters | Contract-level at-least-once, event-ID dedupe, optional aggregate sequence, explicitly authorized replay with side effects suppressed | Blocks reliable event implementation, not semantic review | Implementation |
| OD-100 — DANIEL | Timeline visibility affects Profile, Timeline Events, patient/provider experience | Broad feed; curated category policy; domain-specific manual publishing | Curated, minimal patient view; clinically relevant Provider view; internal/audit facts hidden by default | Patient Timeline UX cannot finalize | Experience launch, not core architecture |
| OD-101 — COMPLIANCE | Isolation/offboarding affects Person, Profile, Evidence, organizations, employers, tenants | Eden-only; logical tenant scope; full delegated tenant model | Eden-only operations with latent scope and no cross-tenant sharing or tenant Protocol publishing | Does not block Eden MVP; blocks external tenant commitments | Future partner launch |
| OD-102 — DANIEL | Design stewardship affects all patient/provider surfaces | Advisory principles; release gate; dedicated design owner | Adopt as product review gate with accessibility/clinical clarity overriding minimalism | Inconsistent experience but no domain-model block | Experience quality |
| OD-103 — ENGINEERING | Rule portability affects Rule Definitions, simulations, and conformance | Custom DSL; decision tables; code adapters; vendor engine | Keep implementation-neutral logical contract and conformance cases until use cases select runtime | Blocks concrete Rules Engine implementation, not current package | Implementation |
| OD-105 — CLINICAL | Provider judgment boundary affects Evidence, Provider Review, Eligibility Decision, Care Plan, and audit | Evidence only; rationale only; both linked | Decision rationale is mandatory; create separate Evidence only when the judgment is independently reusable, with one source reference rather than copied prose | Clinical audit/data model cannot finalize | Clinical implementation |

All owners use the approved owner set. No decision is falsely presented as a
pharmacy or product launch blocker.

## 9. Accepted-decision quality

| Decision | Support | Implementation lock | Quality result |
| --- | --- | --- | --- |
| AD-008 — six contexts | Foundation domain map and Package 003 directive | None; bounded context is semantic, not service topology | Accepted; affected chapters and no-supersession statement added |
| AD-009 — federated rule ownership | Configuration-over-code and domain authority principles | No rule language/vendor/runtime selected | Accepted; supersedes only an informal ownerless-engine assumption |
| AD-010 — immutable Evidence Sets | AD-003 historical context and AD-004 suggestion separation | No storage mechanism selected | Accepted; exact affected chapters linked |
| AD-011 — Domain Event/Timeline separation | Platform Language and Core Object Model already distinguish them | No broker/event-sourcing choice | Accepted; explicitly rejects event-bus-as-record assumption |
| AD-012 — proportional tenant scope | Package directive requires clean future boundary and Eden-first simplicity | Explicitly defers tenant administration | Accepted; no prior decision superseded |

All five are supported architectural consequences, not recommendations that should
be demoted. None locks a database, API, vendor, deployment, or tenancy product.

## 10. Scaling review

| Scale pressure | Conceptual bottleneck | Canon safeguard / required future work |
| --- | --- | --- |
| 500 Medications / many Protocol versions | Product-specific rule/protocol copies | Stable Medication/Formulation concepts, reusable versioned modules/Rule Sets, explicit mappings |
| 100 pharmacies | Product recreation and routing/pricing coupling | Pharmacy configurations and serviceable mappings remain Operations-owned; retail Price stays Commerce-owned |
| 500 provider organizations | Organization/role ambiguity and centralized assignment | Identity-owned Organization/authority plus Operations routing; no organization encoded in Person/Protocol identity |
| 10 million members | Timeline as unbounded operational query store | Timeline is append-only longitudinal index with bounded projections; source domains answer current operational queries |
| Years of Evidence | Copied Evidence in every decision and Recommendation | Evidence Set manifests reference exact versions; lifecycle/retention policies prevent unbounded unnecessary Recommendation context |
| Many rule versions | One central Rules Engine monolith | Federated owner Rule Sets, portable contracts, conformance, simulations, and runtime independence |
| Many event consumers | Universal replay and global ordering | Per-aggregate ordering only, replay authorization, idempotency, side-effect suppression, owner reconciliation |
| Future tenants | Tenant overrides leaking into canonical meaning | Tenant scope remains an authorization/configuration dimension below constitutional and Clinical authority |

No implementation optimization is required now. These boundaries remain coherent
at the stated scale if implementations use partitioned projections, bounded queries,
and retention tiers later without changing the business model.

## 11. Repository hygiene

- No `/private/tmp` path is staged or tracked. Review scripts remain outside Git.
- No raw clinical document, spreadsheet, archive, PDF, private source, or temporary
  script is staged.
- Package 002 has no working-tree or index changes.
- The intended scope is Package 003 plus `README.md`, `STATUS.md`,
  `OPEN-DECISIONS.md`, `canon/00-master-index.md`, and
  `canon/17-decision-log.md`.
- Both Package 003 directives are registered by hash; attachments remain outside Git.
- No production code, SQL, physical schema, API, or implementation dependency exists.

## 12. Final commit recommendation

**APPROVE COMMIT AFTER STAGED-DIFF CONFIRMATION.**

The corrected package is suitable for an additive Package 003 architecture commit.
Open decisions must remain visible and must be resolved before their corresponding
implementation or experience gates, but they do not invalidate the semantic model.
Do not amend Package 001 or Package 002 history.
