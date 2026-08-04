# Product-to-Protocol Matrix

**Status:** Partial; sponsor constraints only

This matrix separates confirmed rules from proposed architecture. Blank or
unresolved cells MUST NOT be interpreted as “not required.”

| Product | Product identity source | Protocol Version | Intake | Labs | Synchronous visit | Medical media | Documents | Evidence status |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Care | SRC-002; exact catalog identity is OPEN — DANIEL | OPEN — CLINICAL | OPEN — CLINICAL | CONFIRMED: lab-free; exact scope is OPEN — CLINICAL | OPEN — CLINICAL | OPEN — CLINICAL | OPEN — CLINICAL | Mixed |
| Optimize | SRC-002; exact catalog identity is OPEN — DANIEL | OPEN — CLINICAL | OPEN — CLINICAL | CONFIRMED: lab-gated; exact tests, timing, and freshness are OPEN — CLINICAL | OPEN — CLINICAL | OPEN — CLINICAL | OPEN — CLINICAL | Mixed |

## Interpretation

`Care is lab-free` currently means no required laboratory gate is asserted for
Care by SRC-002. Package 002 still needs Clinical confirmation of scope and any
non-gating optional lab behavior.

`Optimize is lab-gated` establishes that a lab requirement must be satisfied. It
does not identify a panel, threshold, freshness interval, ordering workflow, or
exception policy; those MUST come from an authoritative registered source or an
explicit Clinical decision.

No other Product rows will be invented before an approved patient-facing Product
catalog and the missing SRC-003 formulary are registered.

## Clinical program source coverage

These are source program names, not yet patient-facing Product identities.

| Source program | Candidate Protocol | Lab evidence | Source status |
| --- | --- | --- | --- |
| Compounded injectable GLP-1 weight loss | Injectable weight-loss protocol | SRC-012 says optional, not required | OPEN — CLINICAL conflict review and OPEN — DANIEL Product mapping |
| Oral semaglutide | Oral semaglutide protocol | Same as injectable in SRC-012 | OPEN — CLINICAL review and OPEN — DANIEL Product mapping |
| GLP-1 microdosing | Microdosing protocol | No labs in SRC-012 and SRC-019 | OPEN — CLINICAL approval and OPEN — DANIEL Product mapping |
| Anti-aging injectables and Metformin | Anti-aging protocol | No mandatory labs, with an incomplete CKD/Metformin exception in SRC-012 | OPEN — CLINICAL |
| Metabolic support and peptides | Metabolic-support protocol | SRC-012 and SRC-013 conflict on six-month monitoring | OPEN — CLINICAL |
| Menopause / HRT | HRT protocol | Age- and treatment-dependent requirements conflict across SRC-005, SRC-012, and SRC-018 | OPEN — CLINICAL |
| Anti-nausea | Ondansetron protocol | Not required | OPEN — CLINICAL because dosing sources conflict |
