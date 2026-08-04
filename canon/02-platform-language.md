# 02 — Platform Language

**Status:** Foundation Draft  
**Owner:** Platform Architecture

## Purpose

Establish a shared vocabulary so clinical, commercial, operational, and
technical teams do not use the same word for different concepts.

## Principles

- Terms describe domain meaning, not database tables or screens.
- A canonical term has one meaning across the platform.
- Similar concepts remain separate when their authority or lifecycle differs.
- Patient-facing language MAY differ from internal canonical language, but the
  mapping MUST be explicit.

## Canonical Objects

| Term | Canonical meaning | Must not be used to mean |
| --- | --- | --- |
| Person | A human identity independent of platform relationship | Login account |
| Account | Authentication and access container for one or more principals | Clinical identity |
| Member | A person with a longitudinal HealthOS relationship | Active subscriber only |
| Patient | A member in the context of a specific clinical care relationship | Every visitor or purchaser |
| Actor | Person or system performing an attributable action | The subject of the action |
| Organization | A legal or operational entity participating in the platform | A user role |
| Care relationship | Governed relationship among a patient and clinical organization or professionals | A single encounter |
| Provider | Credentialed clinical actor operating within verified role, jurisdiction, and scope | Any care-team or support actor |
| Care Team | Operational and care-support relationship; membership alone grants no clinical decision or prescribing authority | Provider authority |
| Encounter | A bounded clinical interaction | Entire treatment history |
| Protocol | Versioned clinical policy defining required and allowed care behavior | Product configuration |
| Evaluation | Process that gathers evidence for an accountable determination | Questionnaire alone |
| Clinical decision | Determination made under clinical authority | Automated suggestion |
| Product | Stable patient-facing care concept | Pharmacy SKU or offer |
| Offering | A product made commercially available in a defined context | The product itself |
| Offer | Versioned terms presented to an eligible audience | Clinical eligibility |
| Price | Versioned monetary amount and currency under defined conditions | Benefit or discount logic |
| Membership | Governed commercial relationship that may grant access and benefits | Medication purchase |
| Billing Option | Versioned selectable membership payment terms, such as cadence and timing | Membership Plan or Billing Arrangement |
| Dunning | Billing-collection state following an unresolved payment failure | Treatment discontinuation or clinical ineligibility |
| Benefit | Versioned entitlement applied under a membership or program | Price itself |
| Order | Request for goods or services after required gates | Prescription |
| Payment authorization | Permission to reserve or later capture funds | Captured payment |
| Prescription | Clinician-authorized medication instruction | Order or pharmacy SKU |
| Formulation | Defined medication composition and dosage form independent of patient-facing Product | Product or pharmacy inventory identity |
| Pharmacy SKU | Pharmacy-specific dispensable package or inventory identity | Product, Formulation, or Prescription |
| Fill | A fulfillment instance against an authorized Prescription | Treatment Plan or Prescription itself |
| Fulfillment | Operational work to deliver an approved good or service | Clinical approval |
| Timeline event | Member-centered, provenance-linked record that something occurred | Domain event transport message |
| Domain event | Immutable statement from an owning domain that a fact became true | Command or request |
| Assertion | Attributed claim that may later be corrected or superseded | Proven fact by default |
| Suggestion | Non-authoritative derived output requiring bounded use or review | Decision |
| Configuration version | Immutable, effective-dated policy representation | Mutable settings blob |

## Relationships

- A Person MAY become a Member without purchasing anything.
- A Member becomes a Patient only within a Care Relationship.
- A Product references clinical and operational capabilities but is independent
  of any Offer, Price, pharmacy SKU, or Membership.
- An Offer MAY reference Products, Prices, and Membership Plans but MUST NOT
  redefine Protocols.
- A Clinical Decision MAY authorize a Prescription; a Prescription MAY permit an
  Order to proceed; neither guarantees Fulfillment.
- Product, Formulation, Pharmacy SKU, Prescription, and Fill have distinct
  identifiers and lifecycles even when an implementation maps them closely.
- A Domain Event MAY produce one or more member-visible Timeline Events, but the
  two records serve different purposes.

## Business Rules

- APIs and schemas MUST use canonical terms unless an explicit adapter maps an
  external vocabulary.
- The unqualified term `status` SHOULD NOT cross a domain boundary; name the
  lifecycle, such as `membershipStatus` or `evaluationStatus`.
- `approved` MUST identify both approving authority and subject of approval.
- `active` MUST define its time basis and lifecycle.

## State Machines

Every lifecycle term is defined in its owning chapter. Shared transition
conventions appear in Chapter 04.

## Events

Event names use past-tense facts in the form `<domain>.<subject>.<fact>.vN`, for
example `clinical.evaluation.completed.v1`.

## Permissions

Changing a canonical term requires review by each affected domain. Terms exposed
in external contracts require a compatibility plan.

## Configuration

Display labels and translations MAY be configured. Canonical identifiers and
semantic meaning MUST NOT vary by tenant or market.

## Acceptance Criteria

- Each object in a new design uses an existing term or proposes a glossary change.
- Authority-distinct concepts are modeled separately.
- External vocabulary mappings are documented at system boundaries.
- Event and state names convey their owning domain and meaning.

## Future Extensions

- Machine-readable vocabulary registry.
- FHIR and partner terminology mapping profiles.
- Localization and health-literacy guidance.

## Anti-Patterns

- A single `User` record acting as person, member, patient, and account.
- A single `Product` record acting as therapy, offer, price, and pharmacy SKU.
- A generic `approved` boolean shared by commercial and clinical workflows.
- Renaming historical terms without compatibility aliases or migration context.

## Open Decisions

- OD-007 — OPEN — CLINICAL
- OD-008 — OPEN — DANIEL
- OD-062 — OPEN — CLINICAL

Definitions and disposition are centralized in the
[Open Decision Register](../OPEN-DECISIONS.md).
