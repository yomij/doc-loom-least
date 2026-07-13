---
name: doc-sync-close
description: Close an existing Doc Loom case after execution or when blocked, paused, cancelled, superseded, or abandoned. Own final acceptance/status, safe docs sync, authority proposals or approved narrow patches, and the declared closure/completion commit.
---

# doc-sync-close

Read `references/shared-protocol.md` for shared identity/status/artifact/commit
rules. Read `references/doc-update-rules.md` only for L1 authority, L3 derived
sync, or an authority-change table. Read `templates/handoff.md` only when
writing a real future resume point.

## Inputs And Workflow

Consume plan goal/non-goals/decisions/acceptance/docs impact, execution/checks/
diff/review risk, required Engineering/Spec aggregate, actual task/fix hashes,
user findings, current artifacts, and relevant authority/derived docs. Missing
case identity returns to the doorway.

1. Choose a shared final-status candidate from evidence and user instruction.
2. Map every criterion to `met`, `partially_met`, `not_met`, `not_verified`, or
   `out_of_scope`; apply the Done Gate.
3. Summarize final changes/checks/unrun work/commits/risks without repeating
   plan or interim narrative.
4. Sync L2 and mechanically safe L3; propose authority or apply only the exact
   approved narrow patch.
5. Write/validate `closure.md` with final status; refresh dashboard and only
   evidenced product state.
6. Stage/inspect/check/commit the declared full-flow closure or Fast-Path
   combined unit; verify title/trailers and clean case scope.
7. Write handoff only for a real future resume point.

## Done Gate

Unqualified `Done` requires:

- every criterion met or explained out of scope;
- required execution evidence, or full verification in closure when optional;
- confirmed deviations and resolved material user findings;
- resolved high review risk;
- for eligible cases, exact baseline, separate Engineering/Spec verdicts,
  aggregate `pass`, finding/re-review disposition, and residual caveats;
- verified planned semantic task/refactor/fix commits, or the reviewed
  Fast-Path green combined unit;
- successful declared closure/completion commit containing final `closure.md`;
- no unexplained case-related worktree change.

`not_verified` needs verification, a user caveat, or non-Done status;
`partially_met` cannot be Done and needs an accepted follow-up for caveated
closure; `not_met` normally blocks/pauses/cancels/supersedes; `out_of_scope`
must explain the boundary. Missing optional execution alone does not downgrade.

Use `Done with Caveats` for a complete main goal with accepted residual risk or
follow-up. Other shared statuses state their specific reason.

## Knowledge And Authority

Classify reusable knowledge as `authority_candidate`, `ADR_candidate`,
`regression_candidate`, `runbook_candidate`, `derived_sync`, `case_local`, or
`none`. Never silently promote it. An unresolved authority candidate is
residual risk and prevents unqualified Done.

Authority defaults to proposal. Apply only an existing-boundary narrow patch
explicitly confirmed by user or exact approved plan and satisfying the update
rules. Structural/high-risk/conflicting/new-area/lifecycle work becomes
governance or a new case.

## Closure And Gates

`closure.md` frontmatter contains `case_id`, final shared `status`, and
`updated_at`; never predict its own commit hash. Uncommitted atomic closure is
pending. Fast-Path may include only the already reviewed green delta; full-flow
closure contains no implementation fix. Commit failure preserves evidence and
blocks unqualified Done.

A skipped/failed dashboard or product-state refresh is recorded but does not
roll back an otherwise valid closure.

- Every ended/paused/blocked/cancelled/superseded case needs closure.
- Never report final closure before writing `closure.md`.
- No concrete authority confirmation -> proposal/wait, not patch.
- Do not change code, tests, scripts, dependencies, lockfiles, CI, acceptance,
  or plan semantics to make closure pass.
- A failed Done Gate condition blocks an unqualified Done report.
- Report Done only after commit success and clean case scope.
