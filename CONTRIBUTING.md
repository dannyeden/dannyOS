# Contributing to the Canon

## Decision hierarchy

When two documents conflict, resolve them in this order:

1. Ratified constitutional principles
2. Ratified Canon chapters
3. Accepted RFCs and decision-log entries
4. Draft Canon chapters
5. Diagrams, schemas, and implementation notes

Conflicts must be resolved explicitly. A newer document does not automatically
override an older one.

## Change classes

| Change | Required process |
| --- | --- |
| Typo, wording, or non-semantic clarification | Direct pull request |
| Additive detail within an existing boundary | Pull request with rationale |
| New capability, object, state, or event | RFC or linked design note |
| Cross-domain contract change | RFC with affected owners |
| Constitutional or clinical-safety change | RFC plus explicit ratification |

## Canon chapter standard

Every chapter must address these sections, or state why one is not applicable:

- Purpose
- Principles
- Canonical Objects
- Relationships
- Business Rules
- State Machines
- Events
- Permissions
- Configuration
- Acceptance Criteria
- Future Extensions
- Anti-Patterns
- Open Decisions

## Writing rules

- State invariants as testable rules using **MUST**, **MUST NOT**, **SHOULD**,
  and **MAY** deliberately.
- Separate clinical authority from commercial configuration.
- Prefer capability language over therapy- or product-specific language.
- Identify the owning domain for every canonical object and event.
- Never describe destructive replacement as an update when history matters.
- Distinguish facts, decisions, recommendations, and generated suggestions.

## Definition of done

A Canon change is complete when terminology is consistent, relationships and
ownership are explicit, relevant states and events are named, permissions and
configuration boundaries are defined, acceptance criteria are testable, links
resolve, and the master index reflects the document's status.

