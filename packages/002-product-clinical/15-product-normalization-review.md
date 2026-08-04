# Package 002 Product Normalization Review

**Source:** SRC-003

**Snapshot date:** 2026-08-03

**Status:** Complete normalization; authoritative approvals pending

## Authority boundary

SRC-003 controls current Eden Pharmacy availability, source names and categories,
formulations, strengths, package sizes, and the pharmacy pricing fields it
contains. It does not control clinical policy, membership-program placement,
state availability, patient claims, final retail pricing, or marketplace display.

The raw workbook is excluded from Git. Its SHA-256 and every exact product row are
preserved in the source manifest and lossless row register.

## Normalization results

| Measure | Count |
| --- | ---: |
| Source product rows | 106 |
| Canonical medication concepts | 50 |
| Formulations | 69 |
| Medication pharmacy configurations | 101 |
| Apparent duplicate groups | 6 |
| Ambiguous groupings | 19 |
| Non-medication items | 1 |
| Canonical medications lacking clinical documentation | 38 |
| Canonical medications blocked from launch | 50 |

The 101 configuration count excludes the one multi-medication kit and collapses
four exact duplicate source rows into shared configurations. Two additional
probable duplicate pairs remain separate because their source fields conflict.

## Launch interpretation

`Pharmacy source ready` means SRC-003 supports the availability and configuration
facts. It does not mean patient-launch-ready. Every medication is currently
blocked by missing Care/Optimize placement, state availability, final retail
pricing, and marketplace visibility. Products without clinical documentation
also require approved protocol coverage before launch.

## Leadership decisions

Daniel or delegated Eden Commercial leadership must resolve:

- OD-086 — Care, Optimize, or multi-program placement;
- OD-087 — patient-facing names and marketplace visibility;
- OD-088 — final retail pricing and discount presentation;
- OD-091 — category launch scope and sequencing; and
- OD-092 — fulfillment-only versus patient-marketplace configurations.

Clinical, Pharmacy, Compliance, and Engineering decisions remain independently
owned and cannot be overridden by commercial configuration.
