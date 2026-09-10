# Skills Layout

Doc Loom Least keeps four discoverable Skills:

| Skill | Role |
|---|---|
| docloom-workflow | Normal development entry: understand, execute, verify, record when useful, and report. |
| review | Separate read-only verification on request or a defined development trigger. |
| grill | Manual, conversational challenge of a claim or assumption. |
| setup-doc-governance | Structural documentation and authority governance. |

The physical groups are lifecycle organization only:

skills/development/docloom-workflow/
skills/assessment/review/
skills/assessment/grill/
skills/governance/setup-doc-governance/

The default entry owns ordinary context checks, authorization boundaries,
execution, verification, optional durable task records, status, continuation,
discovery, and narrow documentation sync. It does not require a phase choice.

Review triggers are defined in
[docloom-workflow](development/docloom-workflow/SKILL.md); a separate pass does
not require another agent. Use grill only when the user asks to challenge a
claim; it never changes files or task state. Use setup-doc-governance for
structural authority, hierarchy, promotion, and historical-status decisions.
Ordinary documentation edits stay with docloom-workflow.

This README is maintenance navigation. Each installed Skill contains its own
routing, authorization, and completion rules and conditionally loads local
resources; none depends on a README at runtime.

Keep automatic discovery enabled. Skill instructions should state only the
constraints that change decisions; do not add a router, runtime, placeholder
domain, or duplicate protocol. Existing legacy case artifacts remain readable
historical evidence. New durable work uses one task.md record when needed.
