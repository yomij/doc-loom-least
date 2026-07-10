---
name: doc-sync-close
description: Sync docs and close a Doc Loom case, including final review evidence and the atomic closure commit when the approved plan requires them. Use after execution, or when a case is blocked, cancelled, paused, superseded, or abandoned.
---

# doc-sync-close

Read `references/shared-protocol.md` for identity, status, artifacts, authority,
and Atomic Commit rules. Read `references/doc-update-rules.md` before any L1
authority, L3 derived, or authority-change-table work.

## Inputs

- Plan Goal, Non-goals, Decisions, Acceptance, Docs Impact, and amendments.
- Existing/required execution evidence, checks, current diff, and `review_risk`.
- For eligible cases: final Engineering/Spec/aggregate review evidence and
  actual task/refactor/review-fix commit mapping.
- User-provided findings; related authority/L3 docs; `case_state.yaml`.
- Governance plans only for existing blocked decisions or governance follow-up.

This skill consumes an existing case. Documented closure without case docs
routes to `docloom-workflow` for minimal identity.

## Status

Use the exact closure status set in shared-protocol.

- `Done`: the consolidated Done Gate below passes, the closure commit succeeds,
  and case-related worktree scope is clean.
- `Done with Caveats`: main goal is complete but residual risk/follow-up remains.
- Any other status records its specific reason in `closure.md`.

## Workflow

1. Choose candidate status from acceptance evidence, user instruction, and
   execution state.
2. Apply the Done Gate and map every criterion to `met`, `partially_met`,
   `not_met`, `not_verified`, or `out_of_scope`.
3. Summarize changes, checks, unrun verification, commits, risks, and knowledge
   impact without repeating the plan or interim narrative.
4. Sync L2 operational docs and mechanically safe L3 derived docs.
5. Handle authority as a proposal or a confirmed narrow patch.
6. Write and validate `closure.md` without predicting its own commit hash; then
   update `case_state.yaml`.
7. Refresh `docs/cases/README.md` only for dashboard-relevant closure and
   `docs/product/current-state.md` only for evidenced product-state change.
   Both remain non-authoritative; invent no product direction.
8. When required, stage only the closure unit, inspect it, run final checks,
   create the declared closure commit, and verify Git metadata/trailers.
9. Write `handoff.md` only for `Paused`, `Blocked`, or another future resume
   point, using `templates/handoff.md`.
10. Report unqualified `Done` only after closure-commit success and clean
    case-related worktree scope.

## Done Gate

Unqualified `Done` requires all of:

- every acceptance criterion is `met` or explained `out_of_scope`;
- required `execution.md` exists; if optional/absent, `closure.md` carries full
  verification evidence;
- any material deviation returned to `plan-confirm` or was explicitly confirmed;
- user-provided material findings affecting acceptance are resolved;
- high `review_risk` has sufficient resolution evidence;
- for eligible cases, the exact review baseline, separate Engineering and Spec
  verdicts, aggregate `pass`, material-finding disposition, and re-review history
  are present, with residual Minor findings/evidence caveats recorded;
- required semantic commits are verified from Git, not case prose, and actual
  task/refactor/review-fix hashes are recorded;
- required closure path and closed state are committed successfully with no
  unexplained case-related worktree changes.

Acceptance handling when the gate cannot pass:

| Result | Handling |
|---|---|
| `not_verified` | Verify, obtain user decision, record a caveat, or choose `Blocked`/`Done with Caveats`. |
| `partially_met` | Not `Done`; only accepted follow-up permits `Done with Caveats`. |
| `not_met` | Usually `Blocked`, `Paused`, `Cancelled`, or `Superseded`; only a changed user goal permits caveated closure. |
| `out_of_scope` | Does not block, but explain the boundary. |

Missing optional `execution.md` alone does not downgrade closure. Closure owns
final acceptance and verification; include prior detail only when needed to
justify status.

## Knowledge And Authority

Classify knowledge as `authority_candidate`, `ADR_candidate`,
`regression_candidate`, `runbook_candidate`, `derived_sync`, `case_local`, or
`none`. Record the proposal/follow-up/sync/local note; never silently promote it.

Check whether the final diff or confirmed decision creates a reusable future
constraint. Explain `case_local`; unresolved `authority_candidate` is residual
risk and requires `Done with Caveats` instead of `Done`.

Default to a proposed authority change in closure. Apply a patch only with
explicit confirmation of the concrete file/boundary and all
`references/doc-update-rules.md` conditions. An approved plan naming those exact
files and boundaries counts. Structural, high-risk, conflict-heavy, new-area,
bridge/archive, or lifecycle authority work becomes a governance batch/new case.

## State And Closure Commit

Write `closure.md` before state:

```yaml
phase: closed
closure_status: Done with Caveats
closure_path: docs/cases/<case-id>/closure.md
last_updated: <timestamp>
```

If state update fails, closure remains evidence truth but the workflow is not
fully synced. For Atomic Commit plans, uncommitted closure is incomplete: the
declared closure commit must contain both closure path and closed state. On
commit failure, preserve recovery evidence, do not report unqualified `Done`,
and remain `doc_syncing` until Git proves the unit.

A skipped/failed dashboard or product-state refresh does not roll back closure;
record why. Consume final `review_risk`; change it only when new closure evidence
changes the assessment, and record its disposition.

## Gates

- Authority patch without concrete confirmation -> wait for user input.
- A case that ends/pauses/blocks/cancels/is superseded requires `closure.md`.
- Never close state before writing closure or modify plan semantics beyond a
  closure link/note.
- Never change code, tests, dependencies, scripts, lockfiles, CI, acceptance, or
  plan semantics to make closure pass; closure commits contain no implementation
  fixes.
- Any failed Done Gate condition blocks an unqualified `Done` report.
