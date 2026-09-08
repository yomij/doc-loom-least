# Complexity-only Review

Use for Complexity-only or the complexity portion of Dual-pass. In
Post-execution, report complexity findings within Engineering without replacing
its correctness checks.

Report only complexity that can be removed while preserving required behavior
and active contracts. Use locatable, replacement-oriented findings:

`path:line: <tag> <what to cut>; <replacement or nothing>.`

Tags: `delete`, `stdlib`, `native`, `yagni`, `shrink`, `governance-bloat`.
The last covers process or artifacts without a real governance need.

State the reviewed scope and findings, or that nothing useful can be cut.
Estimate reduction only when meaningful; omit generic review checklists.
