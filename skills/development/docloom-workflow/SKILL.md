---
name: docloom-workflow
description: Default human entry for persistent Doc Loom development, case status/continuation, and next-slice discovery. Route internally unless another owner is explicitly invoked. Skip explanations, standalone review/grill, and reversible one-turn low/medium work.
---

# docloom-workflow

Own public entry and case identity; do not plan, execute, close, or make users
name stage Skills.

Read when trigger condition is met:

- [Shared protocol](./references/shared-protocol.md): before routing/identity.
- [Loop protocol](./references/loop-protocol.md): candidate/next-slice
  discovery only.

## Start

Run `git rev-parse --show-toplevel`, `git branch --show-current`, and
`git status --short`. Inspect changed paths only for dirtiness, resume, case
match, or baseline. Without Git, continue safe work and record
`git_available: false`.

## Modes

| Intent | Action |
|---|---|
| Status, continuation, ambiguity | Read minimal case/Git evidence; derive status; create or repair nothing. |
| Candidate discovery | Use current product facts, authority/implementation, dashboard/follow-ups, and `loop-protocol.md`; label inference; recommend one candidate or none. Create no case/plan before selection. |
| Direct selected work | Execute normally without a case, preserving project instructions, verification, and reporting. |
| Persistent selected work | Gather or validly skip context, resolve/create identity, and route to its owner. |

Status output gives evidence, next action/decision, and Git effect. Internal
phase/Skill names are diagnostic only.

## Case Identity

Apply the shared two-question model. Create `docs/cases/<case-id>/` only for a
persistence trigger when immediately routing to planning, persisting required
context, or binding needed execution/review/closure evidence. Its owner writes
the first artifact; leave no empty case.

## Routing

First matching condition wins; explicit Skill invocation wins unless its own
gate routes elsewhere.

| Condition | Route |
|---|---|
| Initialize/rebuild/repair docs governance | `setup-doc-governance` |
| Explicit evidence/code/docs/test/design review | `review` |
| Status/ambiguity or unselected discovery | Read-only mode above |
| Multiple cases with no safe choice | Ask for case selection |
| Reversible one-turn low/medium work with no persistence/guarded need | Direct normal execution; no case |
| Resume, authority/conflict, public/high-risk, weak verification, workflow/agent policy | `context-authority` |
| Current approved plan plus execute/continue intent | `tdd-execute` |
| Persistent work with context or valid skip | `plan-confirm` |
| Execution complete or user requests closure/sync | `doc-sync-close` |

`context-authority` may instead require governance, user decision, case
selection, or conflict resolution. Selected slices still follow normal
identity, context, planning, and confirmation.

## Output

Default:

```text
Status:
What I found or completed:
What happens next:
Decision needed:
Local Git effect: <when relevant>
```

Discovery uses the compact candidate table from `loop-protocol.md` plus one
recommendation and decision request.

## Gates

- Status-only and unselected discovery are read-only.
- Persistent work needs a context summary/brief or valid skip before planning;
  direct work does not enter case stages.
- A compact persistent plan may record the current unambiguous execute request
  as approval. Guarded work requires explicit confirmation of the written
  current plan; older approval always needs current intent.
- Do not auto-trigger ad-hoc review or grill; workflow-owned review belongs to
  approved execution.
- Use shared persistence/guarded triggers, never size or medium risk alone.
- Only authorization-binding assumptions become Constraints. Put durable
  support in a triggered brief, handoff, execution, or terminal carrier—not a
  new plan concern or route artifact.
- Derive case status from shared order: existing `closure.md`, else plan
  `final_status`, else execution/plan draft state.
- Do not write plan risk/baseline, add pipeline stages, or call a backend.
