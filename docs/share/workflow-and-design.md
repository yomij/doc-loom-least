+# Workflow and Design

Doc Loom Least uses the smallest useful development loop:

Understand the outcome from current authority and the actual worktree. Execute
within authorization. Verify each success condition. Update affected
documentation. Report the result and residuals.

The normal entry is docloom-workflow. It does not expose phase selection. A
reversible one-turn task needs no case. A task that must survive the turn gets
one task.md with goal, success conditions, constraints and decisions, current
state, next action, evidence, and result.

Business documentation is independent of task-state persistence. business-docs
extracts business rules and sourced decisions from conversations, task outcomes,
or historical evidence without requiring a prior development flow. Development
captures business changes during work and finalizes an archive at closure;
technical-only work needs no empty document. Sources and time scope distinguish
intended rules, observed behavior, and unknowns. Business archives preserve
snapshots and do not establish authority automatically.

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
