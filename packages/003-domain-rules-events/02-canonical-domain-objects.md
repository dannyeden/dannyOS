# Package 003 Canonical Domain Objects

**Status:** Proposed architecture

## Specification convention

Each object below is an independent business specification. `Lifecycle` names
durable business states, not screens. Published definitions are immutable;
transactional records are corrected or superseded without erasing history. Every
object inherits stable identity, provenance, actor context, occurrence/recording
time, sensitivity, and tenant scope where applicable from the Core Object Model.

## Inherited Product and pharmacy boundaries

Package 002 remains authoritative for source normalization. Package 003 fixes the
business ownership boundary without changing its rows or mappings:

| Object | Authoritative owner | Referenced by |
| --- | --- | --- |
| Product and patient-facing medication presentation | Commerce | Clinical, Operations, Intelligence |
| Medication clinical therapy concept | Clinical | Commerce, Operations, Memory |
| Formulation, Additive, and Strength | Clinical | Commerce and Operations fulfillment mappings |
| Package, Pharmacy SKU, and Pharmacy Source | Operations | Commerce, Clinical, Memory |

Patient-facing identity remains independent from pharmacy configuration. A new
pharmacy maps serviceable configurations to existing concepts; it does not recreate
the Product or Medication. Routing and pharmacy economics remain independent from
retail Price. Historical Orders and Fills retain the exact configuration used.
Deprecating a Formulation or SKU prevents future selection but never erases or
implicitly deprecates Medication, Product, Treatment, or historical use.

## Identity objects

### Person

- **Purpose / owner:** Identity; one human identity independent of login, membership, purchase, tenant, or patient role.
- **Lifecycle / versioning / state:** `candidate → resolved → active → restricted | deceased`; merges and splits are reversible resolution decisions, never destructive edits. Demographics are effective-dated assertions.
- **Relationships / events:** Accounts, Profiles, Households, Memberships, Care Relationships, Timeline; emits `identity.person.resolved.v1` and `identity.person-resolution.corrected.v1`.
- **Permissions / configuration:** The person and authorized Identity actors manage permitted attributes; matching policies are versioned and cannot use mutable contact details as canonical identity.
- **Acceptance / future:** One stable identity survives account and tenant changes; supports guardianship, dependents, federation, and external identifiers.

### Account

- **Purpose / owner:** Identity; authentication and access container for principals, not clinical identity.
- **Lifecycle / versioning / state:** `invited → active → locked | suspended → closed`; credentials and assurance methods have independent histories.
- **Relationships / events:** Links principals to Person and Role Assignments; emits `identity.account.activated.v1`, `identity.account.locked.v1`.
- **Permissions / configuration:** Account owner manages permitted security methods; privileged recovery requires policy and audit.
- **Acceptance / future:** Account changes never rewrite Person history; supports passkeys, enterprise federation, and service principals.

### Organization

- **Purpose / owner:** Identity; stable legal or operational participant such as Eden, a provider group, pharmacy, employer, or tenant sponsor.
- **Lifecycle / versioning / state:** `proposed → verified → active → suspended | retired`; classifications and affiliations are effective-dated.
- **Relationships / events:** Role Assignments, Provider Organizations, Pharmacies, tenant scope; emits `identity.organization.verified.v1`.
- **Permissions / configuration:** Authorized organization administrators manage bounded affiliations; verification source and organization type are governed.
- **Acceptance / future:** Legal identity is separate from brand and tenant; supports networks, acquisitions, and partner hierarchies.

### Household

- **Purpose / owner:** Identity; governed relationship among people for access, support, benefits, or communication without implying shared clinical authority.
- **Lifecycle / versioning / state:** `proposed → active → dissolved`; membership relationships are effective-dated and role-specific.
- **Relationships / events:** Persons, guardianships, sponsorship, Profiles; emits `identity.household-membership.changed.v1`.
- **Permissions / configuration:** Each relationship declares authority, consent, subject, and scope; household membership alone grants no record access.
- **Acceptance / future:** Removing a member preserves history; supports dependents, caregivers, and employer households.

## Memory objects

### Profile

- **Purpose / owner:** Memory; current health-state snapshot for one Person, projected from authoritative clinical, identity, membership, and evidence facts.
- **Lifecycle / versioning / state:** `created → active → restricted | archived`; each projection version records source versions and `as-of` time and is rebuildable.
- **Relationships / events:** Person, Evidence, active plans and treatments, Membership dependency, and Health Timeline; emits `memory.profile.rebuilt.v1` rather than claiming ownership of source changes.
- **Permissions / configuration:** Audience-specific Profile projections are purpose-bound; a patient may correct source assertions through their owning workflow, not edit verified clinical state in place.
- **Acceptance / future:** Profile never replaces the Timeline or clinical chart and contains no mutable copied state that can drift; supports primary and clinically independent guest profiles.

### Health Timeline

- **Purpose / owner:** Memory; canonical longitudinal index of member-centered events across clinical, commercial, provider, operational, and intelligence history.
- **Lifecycle / versioning / state:** `open → restricted | archived`; the stream is append-preserving and its projections are rebuildable.
- **Relationships / events:** Person/Member and Timeline Events; emits `memory.timeline-event.appended.v1` and projection-rebuild facts.
- **Permissions / configuration:** Visibility is evaluated per event, actor, purpose, and consent; inclusion and display policies are versioned.
- **Acceptance / future:** Timeline never becomes the write owner of source facts; supports replay, summaries, visualization, and research views.

### Timeline Event

- **Purpose / owner:** Memory; member-centered, provenance-linked representation that something material occurred.
- **Lifecycle / versioning / state:** `recorded → corrected | restricted`; corrections append linked events. Event type contracts are versioned.
- **Relationships / events:** References source Domain Event or canonical object, Evidence, actor, occurrence time, and visibility.
- **Permissions / configuration:** Projection and disclosure policy determine views; a consumer cannot infer access to the source record.
- **Acceptance / future:** Stable source reference and temporal meaning are mandatory; supports derived summaries and external record export.

### Evidence

- **Purpose / owner:** Memory; immutable, attributable observation or assertion that may support a rule or accountable decision.
- **Lifecycle / versioning / state:** `recorded → verified | disputed | superseded | restricted`; original content remains preserved subject to lawful controls.
- **Relationships / events:** Source Artifact, subject, author, Evidence Set, decisions; emits `memory.evidence.recorded.v1`, `.verified.v1`, `.superseded.v1`.
- **Permissions / configuration:** Access never exceeds source permission; evidence type, verification, freshness, and fitness policies are versioned.
- **Acceptance / future:** Evidence is distinguishable from inference and decision; supports appeals, audits, confidence, and research governance.

### Evidence Set

- **Purpose / owner:** Memory; immutable manifest of exact Evidence versions assembled for a defined decision or generation request.
- **Lifecycle / versioning / state:** `assembling → sealed → expired | superseded`; sealed membership cannot change.
- **Relationships / events:** Evidence references, intended use, assembler, policy, decision or Recommendation; emits `memory.evidence-set.sealed.v1`.
- **Permissions / configuration:** Assembly filters honor purpose, freshness, minimization, and conflict policy.
- **Acceptance / future:** Reproduces what was considered without copying source truth; supports redacted and research-safe manifests.

### Medical Media

- **Purpose / owner:** Memory; governed clinical image, video, audio, or scan plus capture and quality metadata.
- **Lifecycle / versioning / state:** `uploaded → processing → usable | rejected | restricted`; transformations are linked derivatives.
- **Relationships / events:** Person, Evidence, capture requirement, Provider Review; emits `memory.medical-media.recorded.v1`, `.quality-assessed.v1`.
- **Permissions / configuration:** Clinical sensitivity, separate consent, retention, and restricted view/download rights apply; marketing reuse requires separate explicit authorization. A Protocol configures media as `required`, `recommended`, `optional`, or `not-applicable`.
- **Acceptance / future:** Original media and immutable capture metadata remain linked while annotations and AI derivatives are separate. A baseline full-body photo is strongly recommended during Profile creation for weight/body-composition and private progress value, but remains optional for unrelated Protocols unless explicitly required. Supports hair, dermatology, progress photos, video, and Provider annotations.

### Document

- **Purpose / owner:** Memory; identifiable source document such as lab report, identity proof, medication label, consent, or referral.
- **Lifecycle / versioning / state:** `received → classified → verified | rejected | superseded | restricted`; extracted values never replace the source.
- **Relationships / events:** Source Artifact, Evidence, verifier, requirement; emits `memory.document.received.v1`, `.verified.v1`.
- **Permissions / configuration:** Document type controls verifier, retention, and disclosure; access is purpose-bound.
- **Acceptance / future:** Hash, source, and verification disposition are retained; supports signatures, external exchange, and extraction assistance.

## Clinical objects

### Questionnaire

- **Purpose / owner:** Clinical; versioned composition of Questions and reusable intake modules for a defined evaluation purpose.
- **Lifecycle / versioning / state:** definition `draft → approved → active → retired`; response instance `started → completed | abandoned | invalidated`.
- **Relationships / events:** Protocol Version, Questions, Evidence, Evaluation; emits `clinical.questionnaire.completed.v1`.
- **Permissions / configuration:** Clinical approves content and branching; localized presentation cannot change clinical semantics.
- **Acceptance / future:** Responses resolve exact question versions; supports adaptive composition and assisted completion.

### Question

- **Purpose / owner:** Clinical; atomic request for a clinically meaningful response with explicit response semantics.
- **Lifecycle / versioning / state:** versioned definition lifecycle; answers are immutable Evidence and may be corrected by new answers.
- **Relationships / events:** Questionnaire modules, answer Evidence, branching Rule Sets.
- **Permissions / configuration:** Clinical owns clinical meaning; content specialists may change approved display text through mapped versions.
- **Acceptance / future:** Stable meaning, response type, units, options, provenance, and applicability are explicit; supports standard terminology mappings.

### Lab Panel

- **Purpose / owner:** Clinical; versioned requested or required collection of biomarkers with timing and specimen semantics.
- **Lifecycle / versioning / state:** definition lifecycle plus order `planned → ordered → collected → resulted | cancelled | failed`.
- **Relationships / events:** Biomarkers, Protocol, lab order, Lab Results; emits `clinical.lab-panel.ordered.v1`, `.completed.v1`.
- **Permissions / configuration:** Clinical owns composition, freshness, and interpretation requirements; laboratory source owns raw result reporting.
- **Acceptance / future:** Panel version used is preserved; supports external labs, home testing, and jurisdiction overlays.

### Lab Result

- **Purpose / owner:** Clinical owns normalized clinical result; Memory preserves source Evidence.
- **Lifecycle / versioning / state:** `received → reconciled → validated | disputed | corrected`; corrections link, never overwrite.
- **Relationships / events:** Biomarker, specimen, laboratory, source Document, Evidence, Evaluation; emits `clinical.lab-result.validated.v1`.
- **Permissions / configuration:** Authorized clinical actors validate interpretation; unit normalization and reference-range policies are versioned.
- **Acceptance / future:** Source value, normalized value, units, reference range, collection time, and correction lineage remain available.

### Biomarker

- **Purpose / owner:** Clinical; stable semantic concept for a measurable clinical observation, independent of laboratory code.
- **Lifecycle / versioning / state:** `proposed → approved → active → retired`; terminology mappings are effective-dated.
- **Relationships / events:** Lab Panels, Results, units, external codes, Rule Sets.
- **Permissions / configuration:** Clinical terminology owners govern meaning and allowed units.
- **Acceptance / future:** Retiring a code does not erase historical results; supports devices, vital signs, and standards mappings.

### Protocol

- **Purpose / owner:** Clinical; versioned policy defining required and allowed care behavior for a population and purpose.
- **Lifecycle / versioning / state:** `draft → review → approved → active → retired | superseded | withdrawn`.
- **Relationships / events:** Products, Questionnaires, Labs, Evidence requirements, Rule Sets, Treatments; emits `clinical.protocol.activated.v1`.
- **Permissions / configuration:** Clinical governance approves; Commerce and tenant configuration cannot weaken it.
- **Acceptance / future:** Every decision resolves the effective Protocol Version; supports regional overlays and comparative improvement.

### Eligibility Decision

- **Purpose / owner:** Clinical for clinical dimension; immutable determination of `eligible`, `ineligible`, `unknown`, or `not-applicable`.
- **Lifecycle / versioning / state:** `requested → evaluating → determined → expired | superseded`; reevaluation creates a new decision.
- **Relationships / events:** Person, Protocol, Rule Decision, Evidence Set, Provider Review; emits `clinical.eligibility.determined.v1`.
- **Permissions / configuration:** Only Clinical policy or authorized Provider judgment determines clinical eligibility.
- **Acceptance / future:** Reasons, rule versions, evidence, authority, validity, and disclosure-safe explanation are reproducible.

### Provider Review

- **Purpose / owner:** Clinical; accountable request and disposition by a credentialed Provider within verified scope.
- **Lifecycle / versioning / state:** `requested → assigned → in-review → completed | declined | expired`.
- **Relationships / events:** Provider Actor Context, Evidence Set, Evaluation, Recommendation, Prescription; emits `clinical.provider-review.completed.v1`.
- **Permissions / configuration:** Provider authority, jurisdiction, credential, and conflict checks are transition guards; Care Team cannot complete it. Within Protocol bounds, the Provider may reinterpret Evidence, alter dose/monitoring, require additional Labs, Medical Media, Questions, or synchronous visits, override a Recommendation, and approve or decline a Prescription.
- **Acceptance / future:** Original recommendation, Provider disposition, rationale, and authority evidence remain linked; supports peer or specialist review.

### Care Plan

- **Purpose / owner:** Clinical; broad patient-centered set of Provider decisions, goals, interventions, responsibilities, and follow-up across one or more medications or health goals.
- **Lifecycle / versioning / state:** `draft → proposed → approved → active → completed | superseded | discontinued`.
- **Relationships / events:** Patient, Care Relationship, Treatment Plans, goals, Provider Review; emits `clinical.care-plan.approved.v1`.
- **Permissions / configuration:** Collaborative input is attributable; required approval follows clinical policy.
- **Acceptance / future:** Plan changes create versions and preserve patient/provider contributions; supports multidisciplinary care.

### Treatment Plan

- **Purpose / owner:** Clinical; patient-specific dosing, monitoring, duration, follow-up, and permitted-change structure for one Treatment scope.
- **Lifecycle / versioning / state:** `draft → recommended → authorized → active → paused | completed | discontinued | superseded`.
- **Relationships / events:** Care Plan, Protocol, Treatment, Prescription, Evidence; emits `clinical.treatment-plan.authorized.v1`.
- **Permissions / configuration:** Authorized Provider controls clinical authorization; Membership eligibility and Billing Arrangement facts may gate initiation or fulfillment but cannot rewrite the plan.
- **Acceptance / future:** Exact protocol, evidence, author, goals, and change rationale are preserved; supports combination and staged therapies.

### Treatment

- **Purpose / owner:** Clinical; patient-specific longitudinal instance of a Medication or other therapy actually undertaken, independent of purchase or fulfillment instance.
- **Lifecycle / versioning / state:** `planned → initiated → active → paused → resumed | completed | discontinued`; history is append-preserving.
- **Relationships / events:** Treatment Plan, Prescriptions, Fills, adherence Evidence, outcomes; emits `clinical.treatment.started.v1`, `.discontinued.v1`.
- **Permissions / configuration:** Clinical actors determine status from evidence; Operations and Commerce cannot delete or falsify it.
- **Acceptance / future:** Dunning does not discontinue Treatment; supports devices, services, behaviors, and non-medication treatment.

### Prescription

- **Purpose / owner:** Clinical; Provider-authorized medication instruction distinct from Product, Order, SKU, and Fill.
- **Lifecycle / versioning / state:** `draft → review → approved → active → exhausted | expired | cancelled | superseded`.
- **Relationships / events:** Provider Review, Patient, Formulation constraints, Treatment Plan, purchase request, Order, and Fills; emits `clinical.prescription.approved.v1`. For the treatment-checkout workflow, approval occurs only after patient selection and payment authorization request clinical review.
- **Permissions / configuration:** Only authorized Providers issue or change; operational substitution stays within explicit bounds.
- **Acceptance / future:** Authority, instructions, constraints, evidence, and amendment lineage are reconstructable; supports e-prescribing partners.

## Commerce objects

### Product

- **Purpose / owner:** Commerce; stable patient-facing care concept independent of Offer, Price, Formulation, and pharmacy SKU.
- **Lifecycle / versioning / state:** `draft → approved → active → hidden | retired`; capability and protocol mappings are versioned.
- **Relationships / events:** Protocols, capabilities, Offerings, patient-facing Medication; emits `commerce.product.activated.v1`.
- **Permissions / configuration:** Commercial owns presentation within clinical/compliance bounds; Product config cannot alter clinical rules.
- **Acceptance / future:** Product continuity survives fulfillment changes; supports programs, bundles, and multiple tenants.

### Membership

- **Purpose / owner:** Commerce; governed longitudinal commercial relationship and universal prerequisite for treatment initiation.
- **Lifecycle / versioning / state:** `pending → active → suspended | cancelled | expired`; Billing Arrangement has the separate `current → payment-due → dunning → current | exhausted` lifecycle.
- **Relationships / events:** Primary Profile, up to two invited clinically independent guest Profiles, Care/Optimize program enrollments, one membership product, monthly or annual Billing Options, Billing Arrangement, Benefits, Orders, and treatment initiation gate; emits `commerce.membership.activated.v1`; the Billing Arrangement emits `commerce.billing-arrangement.entered-dunning.v1`.
- **Permissions / configuration:** Commercial manages approved terms; no transition erases Treatment history or clinical intent.
- **Acceptance / future:** Membership is prerequisite for treatment access. Care and Optimize are parallel program enrollments; Care has no program-level lab gate and Optimize Treatment Plans are lab-gated, without making labs a universal marketplace gate. During dunning existing Treatment continues, no new Prescriptions or refills issue, and resumption requires clinical evaluation. Cancellation is blocked by non-terminal Treatments across attached Profiles unless the primary uses an attributable override-cancel that clears attached Treatment Plans under approved policy.

### Offer

- **Purpose / owner:** Commerce; immutable version of terms presented to an eligible audience in a context and interval.
- **Lifecycle / versioning / state:** `draft → approved → scheduled → active → paused | expired | retired`.
- **Relationships / events:** Product, Price, Benefit, audience Rule Set, Campaign; emits `commerce.offer.published.v1`.
- **Permissions / configuration:** Authorized commercial and compliance actors approve; no Offer overrides Protocol or authoritative eligibility.
- **Acceptance / future:** Presentation can be reconstructed from exact versions; supports channels, tenants, employers, and experiments.

### Price

- **Purpose / owner:** Commerce; versioned monetary amount, currency, basis, and applicability separate from pharmacy economics.
- **Lifecycle / versioning / state:** `draft → approved → scheduled → active → expired | superseded`.
- **Relationships / events:** Offer, Product, tax/fee policy, price source; emits `commerce.price.activated.v1`.
- **Permissions / configuration:** Pricing authority and approval thresholds are explicit; source pharmacy prices are references, not retail authority.
- **Acceptance / future:** Historical orders retain the applied Price Version; supports regional currency and sponsor contribution.

### Order

- **Purpose / owner:** Commerce; financial and fulfillment request distinct from clinical authorization. Patient medication checkout initially requests purchase and payment authorization pending Prescription review.
- **Lifecycle / versioning / state:** `draft → purchase-requested → payment-authorized → clinical-review → releasable → released → fulfilled | partially-fulfilled | cancelled | refunded | failed`; capture timing follows the approved payment policy.
- **Relationships / events:** Person, Offer/Price snapshot, candidate Medication or Product, payment authorization, later Prescription reference, and Fulfillment Requests; emits `commerce.medication-purchase.requested.v1`, `commerce.order.releasable.v1`, and `.refunded.v1`.
- **Permissions / configuration:** Member or authorized actor places; clinical and serviceability gates remain independently authoritative.
- **Acceptance / future:** Purchase request or card authorization never implies charge capture, Prescription approval, Treatment initiation, or fulfillment. Release to Pharmacy requires the approved Prescription and all remaining gates; supports split fulfillment and sponsored payment.

### Campaign

- **Purpose / owner:** Commerce; governed communication or merchandising initiative with audience, content, purpose, and interval.
- **Lifecycle / versioning / state:** `draft → approved → scheduled → active → paused | completed | cancelled`.
- **Relationships / events:** Offers, audience Rule Sets, content versions, Experiments; emits `commerce.campaign.activated.v1`.
- **Permissions / configuration:** Consent, fairness, and compliance gates precede activation.
- **Acceptance / future:** Campaign cannot infer clinical eligibility or expose sensitive reasons; supports multi-channel orchestration.

### Experiment

- **Purpose / owner:** Commerce owns assignment; Analytics owns measurement definitions and evaluation.
- **Lifecycle / versioning / state:** `draft → approved → running → paused → completed | terminated`.
- **Relationships / events:** Campaign/Offer variants, assignment policy, Metric Definitions; emits `commerce.experiment.assigned.v1`.
- **Permissions / configuration:** Risk and fairness review govern eligibility; clinical care cannot be withheld solely for experimentation.
- **Acceptance / future:** Assignment is reproducible and analysis does not rewrite source outcomes; supports adaptive methods after governance.

## Operations objects

### Pharmacy

- **Purpose / owner:** Operations references verified Organization; fulfillment partner with capabilities, jurisdictions, and pharmacy-native configurations.
- **Lifecycle / versioning / state:** `candidate → verified → active → suspended | retired`; capabilities and service areas are effective-dated.
- **Relationships / events:** Organization, formulations, SKUs, Routing Decisions, Fills; emits `operations.pharmacy-capability.changed.v1`.
- **Permissions / configuration:** Pharmacy and authorized Operations actors maintain source facts subject to verification.
- **Acceptance / future:** Pharmacy identity never becomes Product identity; supports many pharmacies and partner networks.

### Fill

- **Purpose / owner:** Operations; one dispensing/fulfillment instance against an active Prescription.
- **Lifecycle / versioning / state:** `requested → gated → routed → accepted → preparing → shipped | delivered | cancelled | failed | held`.
- **Relationships / events:** Prescription, Order, Pharmacy SKU, Shipment, membership hold; emits `operations.fill.held.v1`, `.released.v1`, `.dispensed.v1`.
- **Permissions / configuration:** Operations and Pharmacy act within Prescription and jurisdiction constraints; dunning prevents new/refill issuance and holds affected workflows without rewriting existing Treatment or Prescription history. Resumption reevaluates clinical requirements.
- **Acceptance / future:** Every substitution and route is attributable; supports partial fills, transfers, and pickup.

### Shipment

- **Purpose / owner:** Operations; physical delivery movement independent of Order, Prescription, and Fill.
- **Lifecycle / versioning / state:** `created → dispatched → in-transit → delivered | exception | returned | lost`.
- **Relationships / events:** Fill, carrier, delivery destination, tracking evidence; emits `operations.shipment.delivered.v1`.
- **Permissions / configuration:** Partners see minimum necessary delivery data; notification and exception policies are versioned.
- **Acceptance / future:** Carrier updates are preserved and reconciled; supports cold chain, pickup, and multiple packages.

### Notification

- **Purpose / owner:** Operations; governed delivery attempt of approved content for a defined purpose and recipient.
- **Lifecycle / versioning / state:** `planned → queued → sent → delivered | failed | suppressed | cancelled`.
- **Relationships / events:** Person contact preference, source domain request, content version, delivery provider; emits `operations.notification.delivered.v1`.
- **Permissions / configuration:** Consent, purpose, channel, quiet hours, and sensitive-content policy apply; delivery does not imply comprehension.
- **Acceptance / future:** Content and delivery evidence are reconstructable; supports localization and new channels.

## Intelligence objects

### Recommendation

- **Purpose / owner:** Intelligence; evidence-linked, non-authoritative proposed action or interpretation for a defined audience and use.
- **Lifecycle / versioning / state:** `generated → presented → accepted | modified | rejected | expired`; acceptance creates a separate domain decision.
- **Relationships / events:** Evidence Set, Model/Workflow Version, material instruction or Prompt Version, Protocol context, reviewer, uncertainty, and resulting decision; emits `intelligence.recommendation.generated.v1`, `.disposition-recorded.v1`.
- **Permissions / configuration:** Use-case policy controls evidence, model, audience, validity, and mandatory review; Provider authority remains external.
- **Acceptance / future:** Original output and disposition survive modification. Preserve enough source and generation provenance for audit without requiring indefinite retention of every raw prompt, hidden model internal, or unnecessary sensitive context; supports deterministic, model-based, and human-authored recommendation sources.

## Cross-object acceptance criteria

- No object doubles as another object's identity or lifecycle.
- Every reference that influenced an outcome resolves an immutable version.
- Every state transition names authority, guards, evidence, and emitted event.
- Correction, deprecation, cancellation, and deletion remain distinct.
- Patient-facing labels may vary; canonical meaning and history do not.
