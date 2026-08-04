# Package 003 Repository Governance Review

**Status:** Proposed additive improvements

## Findings

The repository correctly separates Canon from production code, centralizes open
decisions, uses a chapter standard, preserves private-source boundaries, and favors
reviewable Markdown/Mermaid. Package growth now requires clearer artifact classes,
contract governance, and release mechanics.

## Recommended organization

Retain the current structure and add only when artifacts exist:

```text
canon/                 ratified or foundation-wide normative chapters
packages/              bounded architecture work packages and evidence-backed drafts
rfcs/                   cross-domain or constitutional proposals
decisions/              optional detailed ADR records when the central log becomes too large
contracts/              future implementation-neutral event/rule/vocabulary contracts
diagrams/               reusable cross-package Mermaid views
sources/                manifests and public-safe provenance only; private raw sources ignored
reviews/                semantic, contradiction, and ratification reviews
```

Do not move existing files merely to match the target. Relocation requires a link
compatibility plan and should happen only when it improves active maintenance.

## Artifact status and authority

Every document SHOULD declare `Status`, `Owner`, `Last reviewed`, and authority
class. Standard states are `Draft`, `In Review`, `Accepted`, `Ratified`, `Retired`,
and `Superseded`. Evidence registers additionally use `CONFIRMED`, `PROPOSED`, and
`OPEN` for individual claims. Package completion does not ratify Canon content.

## RFC and decision process

- Require an RFC for a new domain, cross-domain capability contract, constitutional
  invariant, clinical-safety boundary, external compatibility promise, or material
  event/rule semantic change.
- Permit linked design notes for additive detail within an accepted boundary.
- Every RFC names affected objects, events, rules, permissions, migrations, owner
  approvals, compatibility class, and Canon changes.
- Accepted outcomes receive a Decision Log entry; rejected and superseded proposals
  remain discoverable.
- Open decisions remain one policy question each. Platform-wide policy should not
  be duplicated as one decision per Product.

## Naming conventions

- Objects use singular Pascal Case in prose; identifiers use stable meaning-free IDs.
- Event contracts use `<domain>.<subject>.<past-fact>.vN`.
- Commands use imperative verbs and are never labeled events.
- State names are aggregate-qualified outside their owner domain.
- Documents use two-digit package order and descriptive kebab-case names.
- Avoid `manager`, `service`, `data`, `record`, or `status` without canonical meaning.

## Diagram conventions

Every diagram declares conceptual, logical, sequential, or state-machine intent;
uses domain-qualified nodes where ownership is ambiguous; defines line semantics;
and links to the owning specification. Mermaid source stays beside the governing
document unless reused across packages.

## Versioning and releases

- Adopt semantic Canon releases after OD-057 resolves the compatibility policy.
- Tag immutable baselines and record commit, date, authority, and migration notes.
- Classify changes as editorial, additive-compatible, behavior-changing, safety-
  critical, or breaking-contract.
- Implementations declare the exact Canon release and documented deviations.
- Published event, rule, vocabulary, and definition versions never rely on a mutable
  `latest` reference for historical outcomes.

## Architecture review

A review packet SHOULD contain problem, authority/source basis, affected domains,
object changes, rule ownership, events, permissions/privacy, lifecycle, compatibility,
open decisions, and conformance evidence. Required reviewers are domain owners plus
Clinical, Compliance, Commercial, Pharmacy, or Engineering authority as affected.

Suggested gates:

1. terminology and ownership;
2. safety, privacy, and authority;
3. lifecycle, evidence, events, and historical reconstruction;
4. cross-domain compatibility and implementation neutrality;
5. source provenance and unresolved-decision completeness;
6. structural/link validation and explicit ratification status.

## Automated checks to add later

- one H1, declared status/owner, and valid internal links;
- unique AD, OD, RFC, source, object, rule, and event identifiers;
- every `OPEN` reference resolves to the central register and owner class;
- Mermaid syntax and event-name convention;
- raw/private source and temporary-file hygiene;
- contract compatibility and conformance examples when machine-readable artifacts begin.

Automation verifies structure, not clinical or architectural truth.

## Governance acceptance criteria

- A reviewer can determine authority, status, owner, sources, and unresolved items.
- Semantic changes have compatibility and downstream impact records.
- No package silently rewrites a prior accepted decision.
- Private sources remain outside Git while hashes and provenance remain reviewable.
- Architecture review stays proportionate: MVP additions do not require premature
  services, schemas, or exhaustive tenant machinery.

## Open decisions

- OD-002 — OPEN — DANIEL
- OD-006 — OPEN — DANIEL
- OD-057 — OPEN — ENGINEERING
