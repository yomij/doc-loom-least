---
name: doc-sync-close
description: Close an existing Doc Loom case after execution or when blocked, paused, cancelled, superseded, or abandoned. Own final acceptance/status, safe docs sync, authority proposals or approved narrow patches, and any declared completion commit.
---

# doc-sync-close

Read when trigger condition is met:

- [Shared protocol](./references/shared-protocol.md): shared identity, status,
  artifact, or commit rules.
- [Document update rules](./references/doc-update-rules.md): L1 authority, L3
  derived sync, or an authority-change table only.
- [Handoff template](./templates/handoff.md): writing a real future resume
  point only.
- [Closure template](./templates/closure.md): writing or validating
  `closure.md`.

## Inputs And Workflow

Consume plan outcome envelope/docs impact, execution or compact completion
checks, required Engineering/Spec aggregate when triggered, actual declared
commit hashes, user findings, current artifacts, and relevant authority/derived
docs. Missing case identity returns to the doorway.

1. Choose a shared final-status candidate from evidence and user instruction.
2. Map every criterion to `met`, `partially_met`, `not_met`, `not_verified`, or
   `out_of_scope`; apply the Done Gate.
3. Summarize final changes/checks/unrun work/commits/risks without repeating
   plan or interim narrative.
4. Sync L2 and mechanically safe L3; propose authority or apply only the exact
   approved narrow patch.
5. Write/validate `closure.md` with final status; refresh dashboard and only
   evidenced product state.
6. If the plan declares a completion commit, stage/inspect/check/commit it and
   verify title/trailers. Otherwise validate complete artifact evidence and
   explain remaining case scope without adding bookkeeping-only Git work.
7. Write handoff only for a real future resume point.

## Done Gate

Unqualified `Done` requires:

- every criterion met or explained out of scope;
- triggered execution evidence, or compact verification in closure when absent;
- confirmed deviations and resolved material user findings;
- resolved high review risk;
- when deep review is triggered, exact baseline, separate Engineering/Spec
  verdicts, aggregate `pass`, finding/re-review disposition, and residual
  caveats;
- every semantic commit explicitly declared by the plan;
- successful closure/completion commit only when the plan declared one;
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
`updated_at`; never predict its own commit hash. Its complete artifact evidence
is terminal unless the approved plan declared a completion commit. A declared
commit remains pending until successful; its failure preserves evidence and
blocks unqualified Done. Closure contains no implementation fix.

A skipped/failed dashboard or product-state refresh is recorded but does not
roll back an otherwise valid closure.

- Every ended/paused/blocked/cancelled/superseded case needs closure.
- Never report final closure before writing `closure.md`.
- No concrete authority confirmation -> proposal/wait, not patch.
- Do not change code, tests, scripts, dependencies, lockfiles, CI, acceptance,
  or plan semantics to make closure pass.
- A failed Done Gate condition blocks an unqualified Done report.
- Report Done after complete closure evidence, every triggered gate and declared
  commit pass, and no case-scope change remains unexplained.
