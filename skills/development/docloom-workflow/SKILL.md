---
name: docloom-workflow
description: Route persistent development work, report case status, resume cases, or recommend the next task. Routine reversible one-turn work needs no case.
---

# Doc Loom Workflow

Read `references/shared-protocol.md` for execution paths, case identity, and
status. Obtain the repository root, branch, and working-tree status, or reuse
a current snapshot.

## Route the Request

Honor an explicitly requested skill; otherwise route by the current need:

| Need | Owner |
|---|---|
| Governance setup or repair | `setup-doc-governance` |
| Explicit assessment | `review` or `grill` |
| Unresolved context, resumption, authority changes or conflicts, or evidence needed for Guarded work | `context-authority` |
| Completed execution or requested closure | `doc-sync-close` |
| Current authorized contract and intent to execute | `tdd-execute` |
| Persistent work with context resolved or context check validly skipped | `plan-confirm` |
| Reversible one-turn work requiring neither persistence nor Guarded assurance | Direct execution without a case |

Apply the shared persistence and assurance rules; medium risk alone requires
neither. Resolve case ambiguity before creating `docs/cases/<case-id>/`, and
create it only when the responsible skill will write required evidence in the
same flow. Each skill owns its stage work.

## Status and Discovery

Keep status checks and discovery read-only. For discovery, read
`references/loop-protocol.md` and recommend one candidate or none. Route a
selected candidate through the same rules as any other request.

Report case status, evidence, next action, and any required decision or Git
effect. Keep routing internal; create no separate routing record.
