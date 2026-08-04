# Package 003 Domain Relationship Diagrams

**Status:** Conceptual and logical views; implementation topology not implied

## Longitudinal relationship

```mermaid
flowchart TD
    Person[Identity: Person] --> Membership[Commerce: Membership]
    Person --> Profile[Memory: current Profile projection]
    Person --> Timeline[Memory: Health Timeline]
    Membership --> AccessGate[Rules: initiation and benefit gates]
    Profile --> Evidence[Memory: Evidence]
    Timeline --> Clinical[Clinical relationship and evaluation]
    Clinical --> Marketplace[Commerce: eligible marketplace view]
    Marketplace --> Product[Commerce: Product and Offer]
    Product --> CarePlan[Clinical: Care Plan eligibility]
    CarePlan --> Selection[Commerce: patient selection and purchase request]
    Selection --> Prescription[Clinical: Prescription Review and approval]
    Prescription --> Fill[Operations: Fill]
    Fill --> Shipment[Operations: Shipment]
    Timeline --> Analytics[Analytics projections]
    Clinical --> Timeline
    Membership --> Timeline
    Marketplace --> Timeline
    Shipment --> Timeline
```

Arrows indicate governed relationships or published facts, not write ownership.
Marketplace presentation does not authorize Treatment, and purchase does not prove
Treatment initiation.

## Bounded-context contracts

```mermaid
flowchart LR
    I[Identity] -->|actor, authority, consent facts| C[Clinical]
    I -->|person and organization references| M[Memory]
    I -->|member identity references| Co[Commerce]
    M -->|authorized Evidence Sets| C
    M -->|authorized Evidence Sets| In[Intelligence]
    C -->|eligibility, prescription, review facts| Co
    C -->|authorized work| O[Operations]
    Co -->|order, membership, benefit facts| O
    O -->|fill, shipment, work outcomes| C
    O -->|fulfillment outcomes| Co
    In -->|recommendations, never hidden decisions| C
    In -->|bounded suggestions| Co
    I -->|material facts| M
    C -->|material facts| M
    Co -->|material facts| M
    O -->|material facts| M
    In -->|material facts| M
```

## Product-to-care-to-fulfillment

```mermaid
flowchart LR
    Product --> Offering --> Offer --> Price
    Product --> ProtocolVersion[Protocol Version]
    ProtocolVersion --> QuestionnaireVersion[Questionnaire Version]
    ProtocolVersion --> LabSet[Lab Requirement Set]
    ProtocolVersion --> RuleSet[Clinical Rule Set]
    EvidenceSet --> RuleDecision[Candidate Rule Decision]
    RuleSet --> RuleDecision
    RuleDecision --> Recommendation[System Recommendation]
    Recommendation --> ProviderReview
    ProviderReview --> EligibilityDecision[Provider Eligibility Decision]
    EligibilityDecision --> CarePlan
    CarePlan --> PurchaseRequest[Medication Purchase Request]
    PurchaseRequest --> PrescriptionReview
    PrescriptionReview --> Prescription
    Prescription --> FulfillmentMapping[Product Fulfillment Mapping]
    FulfillmentMapping --> PharmacyConfiguration
    PurchaseRequest --> Order
    Order --> FulfillmentRelease[Release after Prescription and remaining gates]
    FulfillmentRelease --> Fill
    Prescription --> Fill
    PharmacyConfiguration --> Fill
    Fill --> Shipment
```

## Rule decision and evidence lineage

```mermaid
flowchart TD
    Source[Source artifact or observation] --> Evidence
    Evidence --> EvidenceSet
    RuleVersion --> RuleSetVersion
    RuleSetVersion --> RuleDecision[Candidate Rule Decision]
    EvidenceSet --> RuleDecision
    RuleDecision --> Recommendation
    EvidenceSet --> ProviderReview
    Recommendation --> ProviderReview
    ProviderReview --> ClinicalDecision[Final patient-specific Clinical Decision]
    ClinicalDecision --> TreatmentPlan
    ClinicalDecision --> TimelineEvent
    Recommendation -. preserved with disposition .-> TimelineEvent
```

## Event-to-Timeline projection

```mermaid
sequenceDiagram
    participant Owner as Owning Domain
    participant Contract as Domain Event Contract
    participant Memory
    participant View as Authorized Timeline View
    Owner->>Owner: Commit named state transition
    Owner->>Contract: Publish immutable fact
    Contract->>Memory: Deliver at least once
    Memory->>Memory: Deduplicate and evaluate inclusion policy
    Memory->>Memory: Append source-linked Timeline Event
    View->>Memory: Request purpose-bound projection
    Memory-->>View: Minimized, audience-safe representation
```

## Membership dunning invariant

```mermaid
flowchart LR
    Dunning[Billing Arrangement enters dunning] --> Fact[Commerce publishes dunning fact with Membership reference]
    Fact --> Hold[Clinical and Operations hold new Prescription and refill workflows]
    Fact -. no transition .-> Treatment[Existing Treatment continues]
    Fact -. no mutation .-> Prescription[Prescription remains clinically true]
    Resolved[Membership returns active] --> Review[Clinical requirements are reevaluated]
    Review --> Release[Operations releases only an eligible authorized workflow]
```

## Diagram conventions

- Solid arrows mean a direct canonical relationship or consumed contract.
- Dotted arrows mean supporting, non-authoritative, or explicitly absent mutation.
- Nodes use `<Domain>: <Object>` where ownership could be ambiguous.
- Diagrams are conceptual unless explicitly marked logical or sequential.
- Textual invariants override layout; diagrams never supersede owning specifications.
