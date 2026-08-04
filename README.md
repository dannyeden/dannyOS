# DannyOS Canon

DannyOS is the architecture and platform Canon for HealthOS: a longitudinal
consumer healthcare operating system designed to support durable clinical
relationships, configurable commercial models, and many future care domains.

This repository defines **what must remain true** across implementations. It is
not a deployable application and must not contain production frontend, backend,
or infrastructure code.

## Repository boundary

This repository owns:

- canonical platform principles and language;
- domain and capability boundaries;
- canonical objects, state machines, events, and invariants;
- accepted architecture decisions and RFCs;
- implementation-neutral diagrams, interface schemas, and logical data models.

Implementation repositories own runtime code, deployment configuration,
framework choices, and implementation-specific migrations. Implementations may
extend the Canon, but may not silently contradict it.

## Start here

1. Read the current [repository status](STATUS.md).
2. Read the [Canon master index](canon/00-master-index.md).
3. Use the [platform language](canon/02-platform-language.md) consistently.
4. Resolve or assign work through the
   [Open Decision Register](OPEN-DECISIONS.md).
5. Propose cross-domain or constitutional changes through an
   [RFC](rfcs/README.md).
6. Record accepted decisions in the
   [decision log](canon/17-decision-log.md).

## Status

The Canon is in **Foundation Draft**. Draft content describes the current design
direction but is not immutable. A chapter becomes normative only when the master
index marks it `Ratified`.
