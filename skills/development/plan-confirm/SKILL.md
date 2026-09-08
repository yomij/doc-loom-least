---
name: plan-confirm
description: Write and authorize a Compact or Guarded outcome contract after case identity and context are resolved. Execution owns the implementation path.
---

# plan-confirm

Read `references/shared-protocol.md` for assurance, authorization, and protected
changes. Require case identity, context or valid skip, and an exact baseline;
return missing identity to `docloom-workflow`.

Write `plan.md` using `templates/plan.md`. Its body contains only:

- **Goal:** desired result and purpose.
- **Success Criteria:** claims and required evidence, with close-time columns.
- **Constraints:** guardrails, non-goals, protected effects, and owner mandates.

Keep implementation choices out of the plan. `tdd-execute` chooses tests and
verification; TDD and new tests need no exception mechanism. Explicit required
checks remain Constraints. Supporting context stays inline or in a triggered
brief.

## Authorization

Record risk/assurance, exact pre-execution baseline, version, and approval in
frontmatter. A required requirements artifact must be approved and receive its
declared requirements commit before planning.

Compact may use the current unambiguous execute request as approval. Guarded
requires confirmation of the written current plan; summarize outcome, scope,
Git effects, interruptions, and exclusions in conversation first.
Record approver, time, version, and confirmation, then continue to execution
unless the user holds, revises, or asks for review first.

Contract-semantic or shared protected changes increment the version, return to
`draft`, and clear approval. Adaptive choices and Compact close-time criterion
evidence/terminal metadata do not change the contract.
