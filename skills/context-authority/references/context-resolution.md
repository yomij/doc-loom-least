# Context Resolution

## Case Intent

Choose the task intent before reading broadly.

| Intent | Minimum evidence | Preferred evidence | Default exclusions |
|---|---|---|---|
| Bug / error fix | Reproduction or error clue, related code entry, one test or verification path | Stack trace, failing test, runbook, known issue | Old design drafts |
| Feature | User goal, draft acceptance criteria, relevant authority/contract or explicit absence, adjacent implementation | Spec, API contract, ADR, test strategy | Historical meeting notes |
| Refactor | Target boundary, relevant modules, test coverage entry, non-touch scope | Repo map, dependency graph, ADR, module owner | Stale README |
| Documentation governance | Scope, authority/index state, historical material, blocked decisions | Governance plan, authority index, case evidence, archive | Deep business code unless verifying facts |
| Workflow / skill design | Target skill, upstream protocol, adjacent skill, constitution or authority constraints | Skill design, workflow protocol, ADR, reference skill | Unrelated business code |
| Resume | Case state, handoff/plan/execution/closure, git state | Case docs, branch/worktree, changed files | Unrelated history |
| Incident / operations | Symptom, impact, rollback clue, runbook | Monitoring notes, rollback guide, recent release | Product requirement docs |

Record intent and why it was chosen in the brief or inline context summary.

## Case Resolution Order

Follow the case identity resolution order in `../../_shared/references/shared-protocol.md`.

Resume-from-closed guidance:
- `Done`/`Cancelled`/`Superseded`/`Abandoned`: do not resume by default.
- `Paused`/`Blocked`/`Done with Caveats`: may resume when the user asks and
  the resume condition is satisfied.

Recommended branch shape:

```text
case/<case-id>
```

If `closure.md` exists, do not resume `Done`, `Cancelled`, `Superseded`, or
`Abandoned` by default. `Paused`, `Blocked`, and `Done with Caveats` can resume
when the user asks and the resume condition is satisfied.

## Run Modes

| Mode | New branch | New worktree | Use when |
|---|---:|---:|---|
| `isolated` | Yes | Yes | Large, parallel, or high-risk tasks |
| `branch` | Yes | No | Normal development task |
| `inline` | No | No | Small task, existing branch, temporary work |

`context-authority` only identifies mode. It does not create branches,
worktrees, or case docs.
