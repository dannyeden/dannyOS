# 10 — Eligibility Engine

**Status:** Foundation Draft  
**Owner:** Operations for composition; each policy domain owns its dimension

## Purpose

Provide explainable, versioned eligibility evaluations without collapsing
clinical, regulatory, operational, and commercial authority into one ruleset.

## Principles

- Eligibility is multidimensional, contextual, and time-bound.
- Each dimension is evaluated by its authoritative domain.
- Unknown is not eligible and is not ineligible.
- Composite eligibility cannot override a negative authoritative dimension.

## Canonical Objects

Eligibility Request, Eligibility Dimension, Eligibility Policy Version,
Eligibility Evidence Set, Dimension Result, Composite Result, Reason Code, and
Reevaluation Trigger.

## Relationships

Clinical owns clinical eligibility; Commerce owns offer and membership eligibility;
Operations owns serviceability; Identity or policy services provide jurisdiction
and authority facts. The orchestrator composes results without rewriting them.

## Business Rules

- Results are `eligible`, `ineligible`, `unknown`, or `not-applicable` per dimension.
- Every result records policy version, evidence, evaluated time, validity, and reasons.
- Missing required evidence yields `unknown` unless policy explicitly defines otherwise.
- Clinical ineligibility cannot be overridden by commercial eligibility.
- A material evidence or policy change triggers reevaluation, not mutation.

## State Machines

Evaluation: `requested → gathering → evaluating → complete | incomplete | failed | expired`.
Composite result is derived and rebuildable from immutable dimension results.

## Events

`operations.eligibility-evaluation.completed.v1`,
`operations.eligibility-result.expired.v1`, and domain-owned dimension facts such
as `clinical.eligibility.determined.v1`.

## Permissions

Policy owners control their dimensions. Reason disclosure is audience-specific;
internal clinical reasoning MUST NOT leak through marketplace or marketing APIs.

## Configuration

Policies, reason mappings, validity windows, and reevaluation triggers are
versioned by the authoritative domain. Composite precedence is constitutional.

## Acceptance Criteria

- Consumers can distinguish every eligibility dimension and unknown state.
- Results are reproducible and expire predictably.
- No domain authors another domain's policy.
- Explanations disclose useful reasons without exceeding access authority.

## Future Extensions

Batch precomputation, prospective simulations, partner capability dimensions,
and jurisdictional overlays.

## Anti-Patterns

One `isEligible` flag; treating missing data as false; copying clinical rules into
Commerce; returning sensitive clinical reasons to advertising systems.

## Open Decisions

Home for the federated orchestration contract, standard reason taxonomy, and
latency versus freshness expectations by use case.
