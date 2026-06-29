# Context Resolution

## Case Intent

Choose the task intent before reading broadly.

| Intent | Minimum evidence | Preferred evidence |
|---|---|---|
| Bug / error fix | Reproduction or error clue, related code entry, one test or verification path | Stack trace, failing test, runbook, known issue |
| Feature | User goal, draft acceptance criteria, relevant authority/contract or explicit absence, adjacent implementation | Spec, API contract, ADR, test strategy |
| Refactor | Target boundary, relevant modules, test coverage entry, non-touch scope | Repo map, dependency graph, ADR, module owner |
| Documentation governance | Scope, authority/index state, historical material, blocked decisions | Governance plan, authority index, case evidence, archive |
| Workflow / skill design | Target skill, upstream protocol, adjacent skill, constitution or authority constraints | Skill design, workflow protocol, ADR, reference skill |
| Resume | Case state, handoff/plan/execution/closure, git state | Case docs, branch/worktree, changed files |
| Incident / operations | Symptom, impact, rollback clue, runbook | Monitoring notes, rollback guide, recent release |

Record intent and why it was chosen in the brief or inline context summary.
Exclude material unrelated to the current intent by default.

## Case Resolution Order

Follow the case identity resolution order in `references/shared-protocol.md`.

Recommended branch shape:

```text
case/<case-id>
```

If `closure.md` exists, follow shared protocol Case Resume. Do not resume
`Done`, `Cancelled`, `Superseded`, or `Abandoned` by default. `Paused`,
`Blocked`, and `Done with Caveats` can resume when the user asks and the resume
condition is satisfied.

## Run Modes

Use shared protocol Run Modes. `context-authority` only identifies mode; it does
not create branches, worktrees, or case docs.
