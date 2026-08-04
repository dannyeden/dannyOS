# Clinical Rule Extraction Model

**Status:** Proposed architecture; candidate source population extracted; Clinical approval pending

## Purpose

Define reusable, versioned structures for extracting SRC-004 without converting
documents into product-specific code or inventing absent rules.

## Source-backed definition types

| Definition | Purpose | Minimum provenance |
| --- | --- | --- |
| Protocol Definition and Version | Govern an end-to-end clinical policy release | Source locations, clinical approver, effective interval |
| Intake Module Version | Reusable question and evidence collection unit | Question source, response semantics, applicability |
| Questionnaire Composition | Ordered composition of Intake Modules | Protocol reference, branching source, version |
| Lab Requirement Set Version | Required, conditional, or explicitly absent lab policy | Tests or panels, timing, freshness, conditions, source |
| Visit Requirement Version | Synchronous, asynchronous, or conditional encounter policy | Trigger, modality, authority, source |
| Medical Media Requirement Version | Governed photo, scan, or other clinical media requirement | Media type, views, quality, timing, source |
| Document Requirement Version | Governed non-media document requirement | Document type, verification, timing, source |
| Provider Review Requirement | Required credential, jurisdiction, and review action | Authority and scope source |
| Follow-up Rule Version | Monitoring, renewal, escalation, and cadence policy | Trigger, action, interval, source |
| Contraindication Rule Version | Evidence-linked exclusion or escalation rule | Condition, outcome, authority, source |

## Extraction rules

- Extract the source's exact rule before proposing normalization.
- Record `required`, `conditional`, `optional`, `not required`, and `not stated`
  as distinct values.
- Do not infer clinical equivalence from similar wording across products.
- Reuse a module only after verifying its semantics, authority, and response model
  are genuinely identical.
- Preserve source conflicts and superseded rules.
- A Product references Protocol Versions; it does not own clinical rules.
- Clinical approval is required before an extracted definition becomes `CONFIRMED`.

## Protocol template

Each initial Protocol Version will contain:

1. purpose, population, jurisdiction, and source provenance;
2. eligibility and contraindication rules;
3. Intake Module and Questionnaire Composition references;
4. Lab Requirement Set reference;
5. Visit Requirement references;
6. Medical Media and Document Requirement references;
7. Provider Review Requirement;
8. allowed decisions and override boundaries;
9. Treatment Plan and prescription-review workflow references;
10. follow-up, renewal, monitoring, and escalation rules;
11. events, permissions, effective interval, and supersession;
12. unresolved clinical gaps.

## Synchronous-visit extraction

For every product and protocol, extract whether a synchronous visit is required,
not required, conditional, or not stated; the triggering conditions; allowed
modality; provider qualification; jurisdiction; timing; and source location.

## Photo and document extraction

Photos are Medical Media, not questionnaire attachments. Extract media type,
required views, recency, quality criteria, reviewer, retention classification, and
failure or retake behavior. Documents remain separate and identify verification
requirements. Absence of a documented requirement remains assigned to one of the
five approved owner classes in the Gap Register.

## Current state

SRC-004 and its 18 files are registered. Initial reusable-module, lab, visit, and
document candidates are inventoried in `07-reusable-clinical-modules.md`.

No Protocol Version or clinical rule has been promoted to `CONFIRMED`: source
conflicts, missing effective dates, and absent Clinical approvals remain open.
