# 16 — Analytics

**Status:** Foundation Draft  
**Owner:** Intelligence, with authoritative domain data owners

## Purpose

Enable trustworthy operational, clinical, commercial, and longitudinal analysis
without creating shadow sources of truth or weakening purpose limitations.

## Principles

- Metrics are versioned semantic products with named owners.
- Source domains remain authoritative; analytical models are reproducible projections.
- Clinical outcomes and commercial outcomes remain distinguishable.
- Event-time, effective-time, and processing-time semantics are explicit.
- Collection and use are minimized to an authorized purpose.

## Canonical Objects

Metric Definition and Version, Cohort Definition, Analytical Event, Data Product,
Lineage Record, Quality Assertion, Experiment Analysis Plan, Attribution Model,
and Published Result.

## Relationships

Domain events and governed snapshots feed analytical data products. Metric
versions reference source contracts and lineage. Intelligence may consume
authorized analytical features, but operational write-back uses owning-domain APIs.

## Business Rules

- Every published metric defines numerator, denominator, exclusions, time basis,
  grain, owner, source versions, and known limitations.
- Historical dashboards retain the metric version used at publication time.
- Clinical outcome metrics cannot be silently optimized as commercial proxies.
- Experiment analysis plans are established before outcome inspection when required.
- Identity resolution, deletion, restriction, and consent changes propagate by policy.
- Analytical data MUST NOT be written directly into authoritative domain records.

## State Machines

Metric Version: `draft → validating → approved → active → deprecated | retired`.
Data Product Run: `scheduled → processing → validating → published | quarantined | failed`.

## Events

`intelligence.metric.version-published.v1`,
`intelligence.data-product.published.v1`,
`intelligence.analytics-quality-check.failed.v1`, and
`intelligence.analytical-result.withdrawn.v1`.

## Permissions

Access uses purpose, sensitivity, cohort risk, role, environment, and data-product
policy. Small cohorts and reidentification risk require additional controls.

## Configuration

Metric definitions, cohorts, quality thresholds, schedules, and retention are
versioned. Authorization and deidentification boundaries cannot be weakened by analysts.

## Acceptance Criteria

- A published result is reproducible from versioned inputs and transformations.
- Metric changes do not rewrite historical meaning.
- Data-quality failure prevents or visibly qualifies publication.
- Clinical, commercial, and operational dimensions can be analyzed without conflation.

## Future Extensions

Federated analysis, privacy-preserving measurement, real-world evidence programs,
causal inference governance, and member-facing longitudinal insights.

## Anti-Patterns

Dashboard-defined business logic; mutable metric definitions; raw production-table
queries as contracts; training features with unknown lineage; silent cohort drift.

## Open Decisions

Semantic-layer technology, initial metric governance body, identity-resolution
policy for analytics, and boundaries for secondary data use.
