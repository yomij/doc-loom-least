---
name: tdd-execute
description: Implement an authorized Doc Loom contract, choose appropriate verification, and resolve findings from required reviews. Tests and TDD are optional methods.
---

# Execute and Verify

Read `references/shared-protocol.md` for authorization, artifacts, protected
changes, and commits. Require a non-terminal case, a current authorized contract,
and a baseline. Return missing prerequisites to the responsible skill.

## Implement and Verify

Choose the smallest sufficient verification for the Success Criteria and actual
risk. Decide whether tests are useful, which cases matter, and whether to use
TDD. Record the choice, reason, and results; omitting new tests requires no
exception approval.

Implement within the contract. Adapt routine execution choices; return contract
or protected changes to planning. Preserve explicitly required checks.

Use `templates/execution.md` when a shared artifact trigger applies. Otherwise,
retain a brief completion assessment covering criteria, checks, diff, and scope
for closure. Commit according to user intent, project policy, and Constraints.

## Review and Close

Invoke `review` in `Post-execution` mode for Guarded work, material deviations,
weak evidence, public or authority effects, or an explicit request. Supply the
exact baseline and complete delta. Record separate Engineering and Spec results,
resolve missing evidence, fix findings, and repeat the affected reviews. Other
work uses the completion assessment above.

When criteria are met and required review passes, mark an existing
`execution.md` as `ready_to_close` and route to `doc-sync-close`. Use
`templates/handoff.md` only when a future resume point is needed.
