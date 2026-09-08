---
name: tdd-execute
description: Execute an authorized Doc Loom contract with agent-selected verification, proportional evidence, and triggered review/fixes.
---

# tdd-execute

Follow `references/shared-protocol.md` for authorization, artifacts, protected
changes, and commits. Require a non-terminal case, current authorized contract,
and baseline; return missing prerequisites to their owner.

## Verification

The agent decides whether to add tests, which cases matter, and whether to use
TDD. Choose the smallest verification sufficient for the real risk and Success
Criteria. No new tests needs no exception approval; success still needs evidence.
Record the choice and reason with the results.

Routine choices belong to execution. Contract or protected changes return to
planning; do not weaken explicit verification Constraints.

## Execute And Hand Off

Implement and verify within the contract. Use `templates/execution.md` only
for shared artifact triggers; otherwise retain a compact criterion/check/diff/
scope assessment for closure. Commit according to user/project intent and
Constraints, not for bookkeeping.

Invoke `review` in `Post-execution` mode for Guarded work, material deviation,
weak evidence, public/authority effects, or explicit request. Provide the exact
baseline and complete delta; persist separate Engineering/Spec results and own
missing evidence, fixes, and re-review. Other work uses the compact check.

When criteria are supported and required review passes, mark existing execution
`ready_to_close` and route to `doc-sync-close`. Read `templates/handoff.md` only
for a real future resume point.
