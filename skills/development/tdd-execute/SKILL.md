---
name: tdd-execute
description: Execute an approved Doc Loom case plan using TDD discipline. Use only after plan-confirm has produced an approved plan.
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
7. Update execution docs and plan checkbox progress.
8. Update handoff only when a future resume point exists. Use
   `templates/handoff.md`.
9. Record `review_risk` as a signal only.
10. Check every acceptance criterion.
11. When execution work is complete and closure is the next owner, update
    `case_state.yaml` to `phase: doc_syncing`.
12. Stage or commit only when the plan or user explicitly authorizes it.

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

Record `review_risk` as data, not a recommendation or workflow gate.

| Value | When to set |
|---|---|
| `low` | Local change, no public contract, tests pass. |
| `medium` | New internal feature, moderate scope, main paths covered. |
| `high` | Implementation touches a High-Risk Topic, coverage insufficient, material plan deviation, or authority proposal pending. |

Update when scope changes or new evidence resolves a previous concern. Do not
clear to `low` unless all original high-risk conditions are resolved with
evidence. The closure skill consumes the final value. Do not trigger `review`;
only the user can request it.

## State Update

Update `case_state.yaml` only for phase and routing signals. Markdown execution
evidence is evidence truth. Set `closure_status: open` on phase entry.

When implementation starts:

```yaml
phase: executing
closure_status: open
```

When implementation and acceptance checks are complete and docs sync or closure
is next:

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

Default: do not stage or commit.

Only stage or commit when the plan or user explicitly asks. Stage only
case-related files, preserve unrelated user changes, and record commit hashes
in `execution.md`.

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
