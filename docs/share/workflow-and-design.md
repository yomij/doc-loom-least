+# Workflow and Design

Doc Loom Least uses the smallest useful development loop:

Understand the outcome from current authority and the actual worktree. Execute
within authorization. Verify each success condition. Update affected
documentation. Report the result and residuals.

The normal entry is docloom-workflow. It does not expose phase selection. A
reversible one-turn task needs no case. A task that must survive the turn gets
one task.md with goal, success conditions, constraints and decisions, current
state, next action, evidence, and result.

Review is read-only and triggered by a request or the concrete conditions in
docloom-workflow. A separate pass does not require another agent. Grill requires
explicit challenge intent and stays in conversation. Governance owns structural
authority and historical-status decisions, reusing approval that already covers
the concrete changes. Ordinary documentation edits remain development work.

Current authority outranks implementation evidence, accepted decisions, user
facts in the current task, case records, derived docs, history, and scratch in
that order. Historical case artifacts remain valid evidence but do not define
the new workflow.

The design intentionally has no stage machine, shared protocol, status file,
automatic review phase, numeric scoring gate, fixed handoff age, or global
installation mutation. Add a rule only when real dogfood shows a repeatable
failure and the smallest corrective contract is clear.
