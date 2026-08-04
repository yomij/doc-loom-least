---
name: docloom-workflow
description: Default human entry for persistent Doc Loom development work, case status/continuation, and next-slice discovery. Route internally unless the user explicitly invokes another owner. Skip explanation-only, standalone review/grill, and reversible one-turn low/medium work.
---

# docloom-workflow

Act as the public doorway and case-identity owner; do not plan, execute, close,
or require users to name stage Skills.

Read when trigger condition is met:

- [Shared protocol](./references/shared-protocol.md): before routing/identity.
- [Loop protocol](./references/loop-protocol.md): candidate/next-slice
  discovery only.

## Start

Run `git rev-parse --show-toplevel`, `git branch --show-current`, and
`git status --short`. Read changed paths only for dirtiness, resume, case
matching, or baseline. Without Git, continue safe work and record
`git_available: false`.

## Modes

| Intent | Action |
|---|---|
| Status, continuation, or ambiguity | Read only minimal case/Git evidence; derive status from artifacts; create or repair nothing. |
| Next work/candidate discovery | Use current product facts, authority/implementation, dashboard/follow-ups, and `loop-protocol.md`; infer labeled facts; recommend exactly one candidate or none. Create no case/plan until selection. |
| Direct selected work | Return to normal repository execution with no case; preserve project instructions, verification, and final reporting. |
| Persistent selected work | Gather/skip context as allowed, resolve/create identity, then route to the owning stage. |

Status output gives evidence, next action/decision, and Git effect. Internal
phase/Skill names are diagnostic only.

## Case Identity

Apply the shared two-question model. Create `docs/cases/<case-id>/` only for a
shared persistence trigger and only when immediately routing to planning,
persisting required context, or binding already-needed execution/review/closure
evidence. The routed owner writes the first artifact; leave no empty case.

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

`context-authority` verdicts may instead require governance, a user decision,
case selection, or conflict resolution. A selected next slice still follows
normal identity, context, planning, and confirmation.

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
- Persistent work needs context summary/brief or an explicit valid skip before
  planning; direct work does not enter case stages.
- A compact persistent plan may record the current unambiguous execute request
  as approval. Guarded work requires explicit confirmation of the written
  current plan; older approval always needs current intent.
- Do not auto-trigger ad-hoc review or grill; workflow-owned review belongs to
  approved execution.
- Use shared persistence/guarded triggers, never numeric size or medium risk
  alone.
- Record material routing assumptions in the owning plan/closure, not a route
  artifact.
- Do not write plan risk/baseline, add pipeline stages, or call a backend.
