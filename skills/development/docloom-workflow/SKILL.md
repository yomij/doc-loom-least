---
name: docloom-workflow
description: Entry and routing for persistent development, case status/continuation, and next-slice discovery. Skip explanations and reversible one-turn low/medium-risk work.
---

# docloom-workflow

Own routing and case identity; stage owners do the work. Read
`references/shared-protocol.md` for paths, identity, and status.
Get repo root, branch, and working-tree status; reuse a current snapshot.

Status and unselected discovery are read-only. For discovery, read
`references/loop-protocol.md` and recommend one candidate or none. Selection
then follows the same routing as any other request.

Use explicit Skill intent first. Otherwise route by the current need:

| Need | Owner |
|---|---|
| Governance setup/repair | `setup-doc-governance` |
| Explicit assessment | `review` or `grill` |
| Unresolved context, resume, authority/conflict, or guarded evidence | `context-authority` |
| Execution complete or requested closure | `doc-sync-close` |
| Current authorized contract and execute intent | `tdd-execute` |
| Persistent work with context or valid skip | `plan-confirm` |
| Reversible one-turn work without persistence/guarded need | Normal direct execution, no case |

Apply shared persistence and assurance triggers; medium risk alone is neither.
Create `docs/cases/<case-id>/` only when its owner will write needed evidence
in the same flow. Resolve ambiguity before creating a new case.

Report human status, evidence, next action, and any real decision or Git effect.
Do not create a routing artifact or make the user choose internal stage names.
