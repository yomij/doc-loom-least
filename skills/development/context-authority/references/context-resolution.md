# Context Resolution

Classify intent before reading broadly:

| Intent | Minimum evidence |
|---|---|
| Bug | Symptom/reproduction, related entry, verification path. |
| Feature | Goal/acceptance, relevant authority/contract or explicit absence, adjacent implementation. |
| Refactor | Target boundary, verification coverage, non-touch scope. |
| Docs governance | Scope, authority/index state, blocked decisions/history needed. |
| Workflow/Skill | Target owner, shared/adjacent contracts, constitution/authority constraints. |
| Resume | Current artifacts, handoff when present, Git state. |
| Incident | Symptom, impact, rollback/runbook evidence. |

Record the chosen intent and exclude unrelated material.

Follow shared case-identity order. Terminal closures do not resume by default;
`Paused`, `Blocked`, and `Done with Caveats` require user intent, satisfied
conditions, and a later `execution.md` Resume record.

Select shared run mode (`isolated`, `branch`, or `inline`) but do not create a
branch, worktree, case, or artifact.
