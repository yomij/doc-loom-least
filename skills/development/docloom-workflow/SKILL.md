---
name: docloom-workflow
description: Default human entry for persistent Doc Loom development work, case status/continuation, and next-slice discovery. Route internally unless the user explicitly invokes another owner. Skip explanation-only, standalone review/grill, and one-shot low-risk edits.
---

# docloom-workflow

Act as the public doorway and case-identity owner; do not plan, execute, close,
or require users to name stage Skills.

Read when trigger condition is met:

- [Shared protocol](./references/shared-protocol.md): before routing or
  creating identity.
- [Loop protocol](./references/loop-protocol.md): candidate/next-slice
  discovery only.

## Start

Run `git rev-parse --show-toplevel`, `git branch --show-current`, and
`git status --short`. Read changed paths only when dirtiness, resume, case
matching, or the next baseline needs them. Without Git, continue read-only/docs
work and record `git_available: false`.

## Modes

| Intent | Action |
|---|---|
| Status, continuation, or ambiguity | Read only minimal case/Git evidence; derive status from artifacts; create or repair nothing. |
| Next work/candidate discovery | Use current product facts, authority/implementation, dashboard/follow-ups, and `loop-protocol.md`; infer labeled facts; recommend exactly one candidate or none. Create no case/plan until selection. |
| Persistent selected work | Gather/skip context as allowed, resolve/create identity, then route to the owning stage. |

For status output, give human status, evidence, next action, decision needed,
and relevant local Git effect. Internal phase/Skill names are diagnostic only.

## Case Identity

Follow shared identity order. Create `docs/cases/<case-id>/` only when routing
immediately into planning, persisting a required context brief, or binding
already-needed execution/review/closure evidence. The routed owner writes the
first artifact in the same flow; do not leave an empty durable case.

## Routing

First matching condition wins; explicit Skill invocation wins unless its own
gate routes elsewhere.

| Condition | Route |
|---|---|
| Initialize/rebuild/repair docs governance | `setup-doc-governance` |
| Explicit evidence/code/docs/test/design review | `review` |
| Status/ambiguity or unselected discovery | Read-only mode above |
| Multiple cases with no safe choice | Ask for case selection |
| Resume, authority/conflict, public/high-risk, workflow/agent policy | `context-authority` |
| Verified Fast-Path | `plan-confirm` with compact auto-approval |
| Persistent work with context or valid skip | `plan-confirm` |
| Current approved plan plus execute/continue intent | `tdd-execute` |
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
  planning.
- Current-plan approval continues to execution unless the user holds/revises;
  older approval needs current intent.
- Do not auto-trigger ad-hoc review or grill; workflow-owned review belongs to
  approved execution.
- Fast-Path must satisfy every shared condition.
- Record material routing assumptions in the owning plan/closure, not a route
  artifact.
- Do not write plan risk/baseline, add pipeline stages, or call a backend.
