---
name: tdd-execute
description: Execute an approved Doc Loom case plan using TDD discipline, semantic atomic commits, and the mandatory post-execution review gate for eligible cases. Use only after plan-confirm has produced an approved plan.
---

# tdd-execute

Ordinary one-shot coding tasks and simple document edits do not automatically
enter this skill.

A recommendation is not execution authorization; execute only after
`plan-confirm` has produced an approved plan with a current confirmation log.
Execution authorization means same-turn current-plan approval, execute/continue
intent, valid Fast-Path, or explicit reconfirmation of an older approved plan.

Read when trigger condition is met:

- `references/shared-protocol.md` when: updating case_state.yaml phase,
  checking artifact policy, or resolving authority order.
- `references/tdd-exceptions.md` when: the plan declares TDD Required: No, or
  execution encounters a situation that may require an exception amendment.
- `../../assessment/review/SKILL.md` when: the approved plan declares a
  Post-Execution Review Strategy or the case is eligible for the mandatory
  final review gate.

## Preflight

Before editing, read:

- `docs/cases/<case-id>/plan.md`
- `case_state.yaml`
- Referenced context brief when high risk, resuming, conflict-related, or
  authority/public contract/workflow-related.
- Relevant code and tests.
- Project command and package-manager instructions.

Verify:

- `plan.md` exists.
- `status: approved`.
- `plan_version` exists.
- `approved_by` and `approved_at` exist, and `Confirmation Log` records
  approval for the current `plan_version`.
- If `approved_by: fast-path`, the plan records the verified Fast-Path
  conditions from shared-protocol.md.
- The case is not in an unrecoverable closed status; see shared-protocol.md →
  Case Resume for resumable statuses.
- `TDD Applicability` is declared.
- `Goal`, `Non-goals`, `Decisions`, and `Acceptance Criteria` are clear.
- Workspace changes relative to `base_commit` are explainable, including
  staged, unstaged, and untracked files.
- Current execution authorization exists.
- Eligible cases declare Post-Execution Review and Atomic Commit strategies.
- When the plan requires an atomic plan commit, the approved plan and routing
  state are already committed before implementation starts.

If the user approves the current plan while `plan.md` is still draft, write the
approval, then continue unless the user asks to hold.

If a required check fails, do not execute. Route back to `plan-confirm` unless
the only issue is missing state cache or same-turn approval writeback.

## Workflow

1. Review the plan critically for executable steps, valid commands, test
   strategy, non-goals, and acceptance criteria.
2. Update `case_state.yaml` to `phase: executing` before implementation starts,
   unless the only work is preflight or approval writeback.
3. Red: write or identify the failing test, run the minimal command, and verify
   the failure reason is expected.
4. Green: write minimal implementation and run the target test.
5. Refactor: only planned or necessary refactors; run related tests after each.
6. Quality check with planned or obvious low-risk read-only commands.
7. At each independently valid green task or verified refactor boundary,
   update evidence and create the approved semantic atomic commit.
8. Update execution docs and plan checkbox progress.
9. Update handoff only when a future resume point exists. Use
   `templates/handoff.md`.
10. Record `review_risk` as a signal.
11. Check every acceptance criterion and run final planned verification.
12. For an eligible case, invoke `review` in `Post-execution` mode, persist its
    Engineering, Spec, and aggregate results, and complete the fix/re-review
    loop below.
13. Only after the aggregate result is `pass`, update `case_state.yaml` to
    `phase: doc_syncing` and route to `doc-sync-close`.
14. Stage or commit only within the approved authorization and strategy.

## TDD Rules

Default: write the failing test before implementation.

Do not claim TDD completion unless you observed the test fail for the expected
reason and later pass. If a new test passes on the first run, stop and decide
whether the test is wrong or the behavior already exists.

Allowed Red sources:

- A newly written failing test.
- An existing failing test that directly maps to current acceptance criteria and
  is not unrelated baseline or flaky failure.

Before accepting Red, apply this test quality gate:

- Name a behavior or public contract, not an implementation step.
- Assert observable output, state, persistence, event, side effect, or error.
- Keep one primary failure theme and only the setup needed to understand it.
- Ensure it catches a plausible real regression and survives behavior-preserving
  refactors.
- Do not assert private methods, internal call order, temporary structures,
  non-contract fields, mock existence, or weak coverage-only facts.
- Do not add production test-only methods or invent meaningless Red just to
  satisfy process.
- Mock only system boundaries or slow, external, unstable dependencies whose
  side effects are understood; keep mock data realistic. Prefer integration
  tests when mocks exceed the behavior under test.

## TDD Exceptions

An exception must already be in the approved plan or same-turn confirmed
amendment. It must include alternative verification. See
`references/tdd-exceptions.md`.

If execution changes `TDD Required: Yes` to `No`, stop and return to
`plan-confirm`; this is a material test strategy change.

## Adaptive Execution

Follow the approved plan by default. If adaptive execution is allowed, record
`minor deviation` amendments in `plan.md`:

```md
## Plan Amendments
| When | Change | Reason | User Confirmation | Impact |
|---|---|---|---|---|
```

## Plan Deviation Classification

Compare actual execution against these approved plan boundaries: Goal,
Non-goals, Decisions, Files to Change, Tasks, Acceptance Criteria, TDD
Applicability, risk level, documentation impact, and command/config/dependency
scope. Do not call a change a deviation unless you can name the boundary it
differs from.

| Classification | Meaning | Handling |
|---|---|---|
| `no deviation` | The work remains fully explained by the approved plan. | Continue and record normal execution evidence. |
| `minor deviation` | Low-risk, same-goal, same-responsibility adjustment, such as adjacent fixtures, helper files, or an equivalent local command adjustment. | Record the change and reason in `execution.md`; if adaptive execution is allowed, also record it in `plan.md ## Plan Amendments`, then continue. |
| `material deviation` | Goal, acceptance criteria, file boundary, risk level, documentation impact, TDD strategy, public contract, authority, cross-module boundary, dependency/config, or migration scope changes. | Stop and return to `plan-confirm` for a plan amendment. |
| `hard stop` | The execution would touch a Non-goal, violate a Decision, modify authority docs, or change scripts, dependencies, lockfiles, CI, or test command config without authorization. | Stop and wait for explicit confirmation or a new plan. |

## Execution Evidence

Write or update `execution.md` when:

- TDD is required.
- Code or behavior changes.
- Any `minor deviation`, `material deviation`, or `hard stop` occurs.
- Tests fail, retry, or need failure explanation.
- Material `review_risk` exists.
- The task may need a future resume point with process evidence.

For docs-only, recovery, or trivial config work, skip `execution.md` when there
is no independent execution evidence to preserve and final verification can fit
in `closure.md`. Do not duplicate planned acceptance criteria from `plan.md` as
final evidence; record actual commands, deviations, failures/retries, interim
findings, and resume-critical facts.

Use `templates/execution.md`. Maintain the latest result in the body and keep a
short `## History`; do not create numbered execution directories by default.

Plan checkbox progress may be updated, but semantic changes must be classified
above.

## Review Risk

Record `review_risk` as data. It does not itself trigger ad-hoc review; eligible
cases use the separately defined mandatory Post-execution gate.

| Value | When to set |
|---|---|
| `low` | Local change, no public contract, tests pass. |
| `medium` | New internal feature, moderate scope, main paths covered. |
| `high` | Implementation touches a High-Risk Topic, coverage insufficient, material plan deviation, or authority proposal pending. |

Update when scope changes or new evidence resolves a previous concern. Do not
clear to `low` unless all original high-risk conditions are resolved with
evidence. The closure skill consumes the final value. Outside an approved
Post-execution gate, only the user can request `review`.

## Post-Execution Review

An eligible case is a persistent case that changes production code or tests,
Skill behavior, workflow policy, a public contract, executable configuration,
or user-observable behavior. Explanation-only, status-only, standalone review,
and one-shot non-case work are not eligible. Purely mechanical docs may use a
compact inline Engineering/Spec check when the approved plan says so.

Start the gate only after:

- every planned implementation task is complete;
- required tests and quality checks have passed;
- acceptance criteria have preliminary evidence;
- expected task/refactor commits exist;
- the exact plan baseline resolves and the full case delta is explainable.

Invoke `review` in `Post-execution` mode under the approved plan. Keep the
Engineering and Spec passes isolated and persist, in `execution.md`, the exact
baseline, reviewed commit/working-tree scope, per-axis verdicts, material
findings, evidence gaps, aggregate result, and re-review history.

Handle the aggregate result deterministically:

- `pass`: continue to `doc_syncing`.
- `insufficient_evidence`: collect the missing material evidence and re-run the
  affected axis plus the aggregate gate; do not treat absence as pass.
- `changes_required`: keep ownership in execution. Fix the smallest coherent
  root cause, add or update regression coverage when appropriate, run relevant
  checks, create one atomic `fix:` commit with `Doc-Loom-Step:
  review-fix:<id>`, re-run the affected axis, then re-run the aggregate gate
  against the accumulated final change.

Unresolved Critical or Important findings block `doc_syncing` and unqualified
closure. Minor findings remain visible in execution evidence and residual-risk
assessment.

## State Update

Update `case_state.yaml` only for phase and routing signals. Markdown execution
evidence is evidence truth. Set `closure_status: open` on phase entry.

When implementation starts:

```yaml
phase: executing
closure_status: open
```

When implementation and acceptance checks are complete and docs sync or closure
is next, and the required Post-execution aggregate result is `pass`:

```yaml
phase: doc_syncing
closure_status: open
```

When review risk affects routing or closure judgment:

```yaml
review_risk: high   # low | medium | high
```

`review_risk` records only `low`, `medium`, or `high`. Reasons stay in
`execution.md` or closure evidence. If `case_state.yaml` update fails, do not
roll back completed implementation, but record the failure and do not claim the
workflow is fully synced.

## Commits

Default: do not stage or commit unless the approved plan or current user
instruction authorizes it. Approval of a plan with an Atomic Commit Strategy
authorizes the declared case-scoped plan, task, refactor, review-fix, and
closure commits without repeated confirmation.

For each authorized commit:

1. Choose one coherent, independently valid completion point or root cause.
2. Include its implementation, tests or alternative verification, necessary
   task-local docs, and evidence needed to explain that unit.
3. Stage explicit case-related paths only. Do not use broad staging or include
   unrelated user or other-case changes.
4. Inspect `git diff --cached`, run `git diff --cached --check`, and run the
   relevant task checks before committing.
5. Use the repository `<type>: <summary>` title and the plan's deterministic
   `Doc-Loom-Case` and `Doc-Loom-Step` trailers.
6. Record the resulting task, refactor, or review-fix hash in `execution.md`.

Do not commit an intentional Red state, failed attempt, timestamp-only update,
or checkbox-only change as a semantic completion point. Combine dependent plan
tasks when separating them would create an invalid commit. Keep an independently
verified behavior-preserving refactor separate when practical.

Authorization never includes unrelated files, push, PR, merge, tag, release,
amend, rebase, squash, history rewriting, or a material plan deviation. If a
mixed file cannot be isolated safely, stop rather than create a non-atomic
commit.

## Gates

- No approved plan, no execution. → Route: plan-confirm. Reason: no approved plan found. Required input: user request for plan creation.
- Current plan has no approval log for the current `plan_version`, no execution. → Route: plan-confirm. Reason: current plan approval unconfirmed. Required input: user explicit approval of the current plan.
- Unrecoverable closed case status, no execution without a new case or explicit
  reconfirmation.
- TDD required but no observed expected Red, or unexpected Red failure, no
  implementation.
- TDD exception without alternative verification -> wait for user input.
- Any `material deviation` or `hard stop`, stop and follow Plan Deviation
  Classification.
- Eligible case without declared review/commit strategies, required plan/task
  commits, or a resolvable exact baseline -> return to `plan-confirm` or report
  `insufficient_evidence`; do not close.
- Required Post-execution result other than `pass` -> remain in execution.
