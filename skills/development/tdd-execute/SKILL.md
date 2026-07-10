---
name: tdd-execute
description: Execute an approved Doc Loom case plan using TDD discipline, semantic atomic commits, and the mandatory post-execution review gate for eligible cases. Use only after plan-confirm has produced an approved plan.
---

# tdd-execute

One-shot work and simple docs do not automatically enter this skill. Execute
only with an approved current plan and current authorization: same-turn plan
approval, execute/continue intent, valid Fast-Path, or explicit reconfirmation
of an older plan. A recommendation is not authorization.

Read:

- `references/shared-protocol.md` for state, artifacts, authority,
  authorization, Atomic Commit, and compatibility rules.
- `references/tdd-exceptions.md` when `TDD Required: No` or considering an
  exception amendment.
- `../../assessment/review/SKILL.md` for an eligible or plan-declared
  Post-execution gate.

## Preflight

Read the case plan/state, required context brief, relevant code/tests, and
project command/package-manager instructions. Verify:

- `plan.md` exists with `status: approved`, `plan_version`, `approved_by`,
  `approved_at`, and a matching Confirmation Log entry;
- Fast-Path approval includes its verified conditions;
- the case is not unrecoverably closed;
- Goal, Non-goals, Decisions, Acceptance Criteria, and TDD Applicability are
  executable and clear;
- staged, unstaged, and untracked changes relative to `base_commit` are
  explainable;
- current execution authorization exists;
- eligible cases declare Post-Execution Review and Atomic Commit strategies;
- any required atomic plan commit already contains the approved plan and
  routing state.

If the user approves a still-draft current plan, perform the minimal approval
writeback first and continue unless they ask to hold. Otherwise failed preflight
returns to `plan-confirm`, except a missing state cache may be repaired locally.

## Workflow

1. Critically check steps, commands, tests, non-goals, and acceptance.
2. Set `phase: executing` before implementation (not for preflight-only or
   approval-writeback-only work).
3. Red: run the smallest behavior test and observe the expected failure.
4. Green: implement minimally and pass the target test.
5. Refactor only as planned/necessary; rerun related tests after each change.
6. Run planned and obvious low-risk quality checks.
7. At each independently valid green task or verified refactor completion,
   update evidence and create its authorized semantic commit.
8. Update plan progress and execution evidence; write handoff only for a future
   resume point, using `templates/handoff.md`.
9. Maintain `review_risk`; verify every acceptance criterion and final check.
10. For eligible work, invoke `review` in Post-execution mode, persist both axes
    and aggregate result, and own the fix/re-review loop.
11. Only after aggregate `pass`, set `phase: doc_syncing` and route to
    `doc-sync-close`.
12. Stage/commit only within the approved strategy and shared authorization.

## TDD Rules

Default: write the failing test first. Claim TDD only after observing the
expected failure and later pass. If a new test passes immediately, determine
whether the behavior already exists or the test is wrong before continuing.

Allowed Red: a new failing test, or an existing non-flaky failure that directly
maps to current acceptance. Before accepting it:

- name behavior/public contract and assert observable output, state,
  persistence, event, side effect, or error;
- keep one failure theme and enough setup to understand it;
- catch a plausible regression and survive behavior-preserving refactors;
- avoid private methods, call order, temporary/non-contract fields, mock
  existence, coverage-only assertions, and production test-only hooks;
- mock only understood system boundaries or slow/external/unstable dependencies,
  with realistic data; prefer integration tests when mocks dominate.

## TDD Exceptions

The approved plan or a same-turn confirmed amendment must declare the exception
and alternative verification; see `references/tdd-exceptions.md`. Changing
`TDD Required: Yes` to `No` is material: stop and return to `plan-confirm`.

## Deviations

Judge actual work against Goal, Non-goals, Decisions, Files, Tasks, Acceptance,
TDD, risk, docs impact, and command/config/dependency scope. Name the changed
boundary before classifying it.

| Class | Handling |
|---|---|
| `no deviation` | Continue; record normal evidence. |
| `minor deviation` | Low-risk, same-goal/responsibility adjustment. Record in `execution.md`; when adaptive execution is allowed, also append `plan.md ## Plan Amendments` with When, Change, Reason, User Confirmation, Impact. |
| `material deviation` | Goal, acceptance, files, risk, docs, TDD, public contract, authority, cross-module, dependency/config, or migration scope changed. Stop for a confirmed plan amendment. |
| `hard stop` | A Non-goal/Decision would be violated, or unapproved authority, script, dependency, lockfile, CI, or test-command config would change. Stop for explicit confirmation/new plan. |

## Execution Evidence

Write/update `execution.md` for TDD, code/behavior change, any deviation, test
failure/retry, material `review_risk`, or continuity evidence. Docs-only,
recovery, or trivial config may skip it when final verification fits in
`closure.md`.

Use `templates/execution.md`; keep current results in the body and a short
History, not numbered execution directories. Record actual commands,
deviations, failures/retries, interim findings, task/fix hashes, and
resume-critical facts—not a copy of planned acceptance. Checkbox progress may
change without changing plan semantics.

## Review Risk

`review_risk` is data, not an ad-hoc review trigger:

| Value | Use |
|---|---|
| `low` | Local change, no public contract, tests pass. |
| `medium` | Internal feature, moderate scope, main paths covered. |
| `high` | High-Risk Topic, insufficient coverage, material deviation, or pending authority proposal. |

Update as evidence changes; do not clear to low until original high-risk causes
are resolved. Closure consumes the final value. Outside an approved
Post-execution gate, only the user may request `review`.

## Post-Execution Review

Eligible cases persistently change production code/tests, Skill behavior,
workflow policy, public contract, executable config, or user-observable
behavior. Explanation/status-only, standalone review, and one-shot non-case work
are ineligible. Purely mechanical docs may use a compact inline Engineering/Spec
check only when the approved plan says so.

Start only when tasks, required tests/checks, preliminary acceptance evidence,
expected task/refactor commits, exact plan baseline, and explainable complete
delta are all present.

Invoke `review` under the approved plan. Keep Engineering and Spec isolated and
persist exact baseline, reviewed commits/worktree, verdicts, findings, evidence
gaps, aggregate result, and re-review history. Consume the aggregate contract
owned by `review`:

- `pass`: proceed to `doc_syncing`.
- `insufficient_evidence`: collect material evidence, rerun the affected axis,
  then aggregate again; absence never passes.
- `changes_required`: remain in execution; fix one smallest coherent root cause,
  add/update regression coverage when appropriate, run relevant checks, commit
  one atomic `fix:` unit with `Doc-Loom-Step: review-fix:<id>`, rerun the affected
  axis, then aggregate against the accumulated final delta.

Unresolved Critical/Important findings block `doc_syncing` and unqualified
closure. Keep Minor findings visible for residual-risk judgment.

## State

State carries phase/routing; Markdown carries evidence. On phase entry keep
`closure_status: open`:

```yaml
# implementation begins
phase: executing
closure_status: open

# acceptance complete and required Post-execution result is pass
phase: doc_syncing
closure_status: open
```

`review_risk` accepts only `low`, `medium`, or `high`; reasons live in evidence.
If state update fails, preserve completed implementation, record the failure,
and do not claim the workflow fully synced.

## Commits

Do not stage/commit without current user or approved-plan authorization. Follow
the full shared Atomic Commit Contract and authorization exclusions. For each
authorized task/refactor/review-fix commit:

1. Select one independently valid completion point/root cause.
2. Include implementation, tests/alternative verification, necessary local docs,
   and explanatory evidence for that unit.
3. Stage explicit case paths only; isolate unrelated user/other-case changes.
4. Inspect `git diff --cached`, run `git diff --cached --check`, and run relevant
   checks.
5. Use `<type>: <summary>` plus deterministic `Doc-Loom-Case` and
   `Doc-Loom-Step` trailers.
6. Record the resulting task/refactor/review-fix hash in `execution.md`.

Combine dependent work when separation would create an invalid commit; keep an
independently verified refactor separate when practical. If a mixed file cannot
be isolated safely, stop.

After separately authorized amend/rebase/squash/history rewrite, prior hashes
and Post-execution evidence are stale. Revalidate the exact baseline and range,
refresh commit mappings, and rerun final Engineering and Spec before closure.

## Gates

- No approved plan or matching current-version approval log -> `plan-confirm`.
- Unrecoverable closed case needs a new case or explicit reconfirmation.
- Required TDD without expected Red, unexpected Red, or an exception without
  alternative verification -> no implementation.
- Material deviation/hard stop -> apply Deviations handling.
- Eligible case missing strategies, required plan/task commits, or resolvable
  exact baseline -> return to `plan-confirm` or report
  `insufficient_evidence`; never close.
- Required Post-execution result other than `pass` -> remain in execution.
