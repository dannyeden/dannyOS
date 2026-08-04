# Package 003 Bounded Contexts

**Status:** Proposed architecture

## Boundary contract

A bounded context owns meaning and mutation authority, not necessarily a service,
database, team, or deployment. The initial modular monolith may host every context,
but module boundaries MUST remain enforceable. A context references another
context's stable identifiers and published facts; it MUST NOT depend on the
producer's tables, private states, or internal rule representation.

## Identity

| Concern | Definition |
| --- | --- |
| Purpose | Establish who people and organizations are, who is acting, and under what authority, relationship, consent, and jurisdiction. |
| Responsibilities | Identity resolution; accounts; organizations; households; role, credential, delegation, consent, and care-team relationships. |
| Objects owned | Person, Account, Organization, Household, Actor Context, Role Assignment, Consent Grant, Care Team. |
| Objects referenced | Profile ID, Membership ID, Care Relationship ID, Provider Review ID, Timeline ID. |
| Events emitted | PersonResolved, OrganizationRegistered, RoleAssigned, ConsentChanged, HouseholdMembershipChanged. |
| Events consumed | MembershipActivated, CareRelationshipEstablished, ProviderReviewRequested. |
| Rules owned | Identity matching, authentication assurance, role/delegation validity, consent, actor authority, organization affiliation. |
| Permissions | Identity administrators manage resolution; organizations manage bounded affiliations; people manage permitted profile and consent choices; credential authority is separately verified. |
| Future extension points | Federated identity, employer sponsorship, guardianship, dependents, partner credential registries. |
| Anti-patterns | One `User` object; email as Person ID; Care Team implying Provider authority; tenant-defined clinical roles. |

## Memory

| Concern | Definition |
| --- | --- |
| Purpose | Preserve what was observed, asserted, decided, and experienced over time with provenance and correction lineage. |
| Responsibilities | Health Timeline, Evidence, source artifacts, documents, medical media, messages, assertions, correction and visibility projections. |
| Objects owned | Profile, Health Timeline, Timeline Event, Evidence, Evidence Set, Source Artifact, Medical Media, Document, Message Record, Assertion. |
| Objects referenced | Person, Membership, Clinical Decision, Order, Shipment, Suggestion, Protocol Version. |
| Events emitted | EvidenceRecorded, TimelineEventAppended, EvidenceSuperseded, MediaRecorded, DocumentVerified. |
| Events consumed | Material facts from all domains that require member-centered longitudinal representation. |
| Rules owned | Provenance completeness, evidence immutability, correction linkage, timeline inclusion, visibility, disclosure, retention-policy application. |
| Permissions | Purpose-bound read access; authors cannot erase prior assertions; member and provider views are distinct; lawful restriction is explicit. |
| Future extension points | Record exchange, research views, longitudinal summaries, external clinical formats, confidence and fitness-for-use. |
| Anti-patterns | Mutable medical history; event bus as patient record; one timeline visibility level; AI summary as source evidence. |

## Clinical

| Concern | Definition |
| --- | --- |
| Purpose | Govern evaluation, eligibility, professional judgment, care planning, treatment, prescribing, and monitoring. |
| Responsibilities | Protocols; questionnaires; labs; evaluations; Provider Reviews; Clinical Decisions; Care and Treatment Plans; Treatments; Prescriptions. Care Team may prepare and route work but cannot assume Provider authority. |
| Objects owned | Care Relationship, Protocol, Questionnaire, Question, Lab Panel, Lab Result, Biomarker, Evaluation, Eligibility Decision, Provider Review, Care Plan, Treatment Plan, Treatment, Prescription. |
| Objects referenced | Person, Membership fact, Evidence Set, Product, Formulation, jurisdiction and Provider authority facts. |
| Events emitted | QuestionnaireCompleted, LabsValidated, EligibilityDetermined, ProviderReviewCompleted, CarePlanApproved, TreatmentStarted, PrescriptionApproved. |
| Events consumed | Membership facts, EvidenceRecorded, jurisdiction/authority changes, fulfillment and adherence facts, Intelligence suggestions. |
| Rules owned | Clinical eligibility, age/BMI, contraindications, lab/intake/media requirements, visit modality, provider review, renewal, monitoring, prescribing safety. |
| Permissions | Care Team may validate labs within approved verification policy, request questionnaires, prepare charts, recommend for Provider review, route or restart administrative work, pause workflows or Treatments where policy permits, and escalate clinical questions. Only an authorized Provider may make final clinical eligibility decisions, reinterpret required evidence, override recommendations or clinician holds, restart after a clinically material gap, modify patient-specific plans, or approve or decline Prescriptions. |
| Future extension points | New care programs, devices, referrals, external clinicians, collaborative planning, clinical quality programs. |
| Anti-patterns | Product-owned protocol forks; checkout determining clinical eligibility; editable decision history; suggestion treated as prescription. |

## Commerce

| Concern | Definition |
| --- | --- |
| Purpose | Govern what is offered, priced, purchased, funded, and entitled without defining clinical permission. |
| Responsibilities | Products, membership, program enrollments, benefits, offers, prices, campaigns, experiments, marketplace publication, orders, payment intent. |
| Objects owned | Product, Offering, Membership, Benefit Grant, Offer, Price, Campaign, Experiment, Marketplace Publication, Order, Payment Intent. |
| Objects referenced | Person/Member, clinical and operational eligibility outcomes, pharmacy configuration references, fulfillment status. |
| Events emitted | MembershipActivated, BillingArrangementEnteredDunning, OfferPublished, OrderPlaced, MedicationPurchaseRequested, PaymentCaptured. |
| Events consumed | EligibilityDetermined, ProductAvailabilityChanged, ShipmentDelivered, jurisdiction facts. |
| Rules owned | Membership and benefits, Care or Optimize enrollment, billing options, attached-profile dependency, pricing, offer eligibility, campaigns, marketplace visibility, purchase authorization, capture, and refunds. |
| Permissions | Commercial administrators manage approved configuration; no commercial actor may weaken clinical policy or issue prescriptions. |
| Future extension points | Employers, sponsorship, insurance-adjacent benefits, additional currencies, partner marketplaces. |
| Anti-patterns | Offer redefining protocol; pharmacy price treated as retail Price; purchase implying prescription; SKU displayed as Product. |

## Operations

| Concern | Definition |
| --- | --- |
| Purpose | Coordinate accountable work and fulfillment across people, organizations, partners, and time. |
| Responsibilities | Workflows, work items, routing, pharmacy/provider assignment, fulfillment, shipment, notifications, service levels, exceptions. |
| Objects owned | Workflow Instance, Work Item, Routing Decision, Pharmacy, Provider Organization Assignment, Fulfillment Request, Fill, Shipment, Notification, Operational Exception. |
| Objects referenced | Product/formulation mappings, Prescription, Order, Membership and Billing Arrangement facts, Person contact preference. |
| Events emitted | WorkAssigned, PharmacyRouted, FillHeld, FillReleased, ShipmentDispatched, ShipmentDelivered, NotificationDelivered. |
| Events consumed | Orders, prescriptions, membership dunning, provider-review requests, consent/contact changes. During dunning Operations holds new Prescription and Fill workflows; it never rewrites existing Treatment. |
| Rules owned | Routing, serviceability, work priority, fill holds, partner capability, notifications, escalation, operational retries. |
| Permissions | Operations may coordinate authorized work but cannot create clinical authorization or alter commercial terms. Partner access is scoped to assigned work. |
| Future extension points | Multiple pharmacies, provider groups, logistics partners, regional operations, automated reconciliation. |
| Anti-patterns | Workflow status replacing aggregate states; pharmacy routing changing prescription; notification delivery implying consent. |

## Intelligence

| Concern | Definition |
| --- | --- |
| Purpose | Produce governed, evidence-linked recommendations, summaries, and derived assertions without taking hidden authority. |
| Responsibilities | Evidence assembly, recommendation generation, model/prompt governance, review, feedback, evaluation, safety and drift monitoring. |
| Objects owned | Intelligence Use Case, Model Release, Prompt/Workflow Version, Generation Request, Recommendation, Suggestion, Derived Assertion, Review Decision, Evaluation Result. |
| Objects referenced | Authorized Evidence Set, Protocol Version, domain decision and outcome references. |
| Events emitted | RecommendationGenerated, AIRecommendationAccepted, SuggestionRejected, ModelReleaseActivated, SafetySignalDetected. |
| Events consumed | EvidenceRecorded, ProtocolActivated, outcome facts, review dispositions, authorized feedback. |
| Rules owned | Use-case eligibility, evidence access, model routing, generation constraints, review requirements, release safety, monitoring. |
| Permissions | Source permissions flow through; reviewers need domain authority; Intelligence cannot grant itself write or decision authority. |
| Future extension points | Multimodal assistance, provider confidence support, patient summaries, privacy-preserving research and learning. |
| Anti-patterns | Universal chatbot; autonomous prescribing; unversioned prompts; output silently written as fact; confidence as authorization. |

## Cross-context dependency rule

Dependencies point toward published contracts, not implementation. Process managers
may coordinate outcomes but own only their workflow state. Shared execution
capabilities—rules, events, search, analytics, and notification delivery—MUST retain
the policy ownership and semantic authority of the originating domain.

## Open decisions

- OD-096 — OPEN — DANIEL
