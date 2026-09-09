---
name: plan-confirm
description: Define and obtain authorization for a Compact or Guarded outcome contract once case identity and context are resolved.
---

# Define and Confirm a Plan

Read `references/shared-protocol.md` for assurance, authorization, and protected
changes. Require case identity, resolved context or a valid skip, and an exact
baseline. Return missing identity to `docloom-workflow`.

## Write the Contract

Use `templates/plan.md`. Keep the body to three sections:

- **Goal:** desired result and purpose.
- **Success Criteria:** verifiable claims and required evidence; leave Status
  and Evidence columns for closure.
- **Constraints:** guardrails, non-goals, protected effects, and owner mandates.

Leave implementation and verification choices to `tdd-execute`, including
whether to add tests or use TDD. Record explicitly required checks as
Constraints. Keep supporting context in conversation or a required brief.

Record risk, assurance, exact pre-execution baseline, version, and approval in
frontmatter. If a requirements artifact is required, it must be approved and
receive any declared requirements commit before planning.

## Confirm and Continue

For Compact, the current unambiguous execution request may serve as approval.
For Guarded, summarize outcome, scope, Git effects, interruptions, and exclusions,
then obtain confirmation of the written current plan.

Record approver, time, version, and confirmation. Continue to execution unless
the user asks to hold, revise, or review first.

Changes to contract semantics or shared protected effects require a new version,
`draft` status, and cleared approval. Execution choices and Compact closure
evidence or terminal metadata do not change the contract.
