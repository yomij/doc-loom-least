---
name: tdd-execute
description: Execute an approved Doc Loom case plan using TDD discipline. Use only after plan-confirm has produced an approved plan.
---

# tdd-execute

Ordinary one-shot coding tasks and simple document edits do not automatically
enter this skill.

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
- `approved_by`, `approved_at`, and `Confirmation Log` refer to the current
  `plan_version`.
- The case is not in an unrecoverable closed status; see shared-protocol.md →
  Case Resume for resumable statuses.
- `TDD Applicability` is declared.
- `Goal`, `Non-goals`, `Decisions`, and `Acceptance Criteria` are clear.
- Workspace changes relative to `base_commit` are explainable, including
  staged, unstaged, and untracked files.

If the user approves and asks to execute in the same turn while `plan.md` is
still draft, perform the minimal approval writeback first, then continue.

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

Avoid testing anti-patterns: do not assert mock existence as behavior, do not
add production test-only methods, do not mock dependencies without understanding
side effects, and keep mock responses aligned with real structures. Prefer
realistic integration tests when mocks exceed the behavior under test.

## TDD Exceptions

An exception must already be in the approved plan or same-turn confirmed
amendment. It must include alternative verification. See
`references/tdd-exceptions.md`.

If execution changes `TDD Required: Yes` to `No`, stop and return to
`plan-confirm`; this is a material test strategy change.

## Adaptive Execution

Follow the approved plan by default. If the plan or user explicitly allows
adaptive execution, record low-risk same-goal amendments in `plan.md`:

```md
## Plan Amendments
| When | Change | Reason | User Confirmation | Impact |
|---|---|---|---|---|
```

Continue only for low-risk changes within the same goal and responsibility
boundary, such as adjacent fixtures, helper files, or minor command adjustment.

Stop and return to `plan-confirm` for goal changes, acceptance changes, risk
changes, public contract, authority impact, cross-module boundary, dependency
or config changes, new migration, or TDD strategy changes.

## Execution Evidence

Write or update `execution.md` when:

- TDD is required.
- Code or behavior changes.
- Plan deviation occurs.
- Tests fail, retry, or need failure explanation.
- Material `review_risk` exists.
- The task may need resume.

Use `templates/execution.md`. Maintain the latest result in the body and keep a
short `## History`; do not create numbered execution directories by default.

Plan checkbox progress may be updated, but do not change plan goals, command
semantics, acceptance criteria, TDD strategy, Decisions, or file scope.

## Review Risk

Record `review_risk` as data, not a recommendation or workflow gate.

| Value | When to set | Meaning |
|---|---|---|
| `low` | Local change, no public contract, tests pass. | Low risk. |
| `medium` | New internal feature, moderate scope, main paths covered. | Medium risk. |
| `high` | Implementation touches a High-Risk Topic, coverage insufficient, material plan deviation, or authority proposal pending. | High risk. |

Update when scope changes or new evidence resolves a previous concern. Do not
clear to `low` unless all original high-risk conditions are resolved with
evidence. The closure skill consumes the final value. Do not trigger `review`;
only the user can request it.

## State Update

Update `case_state.yaml` only for phase and routing signals. Markdown execution
evidence is evidence truth.

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
phase: executing
review_risk: high
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
- Unconfirmed current `plan_version`, no execution. → Route: plan-confirm. Reason: plan version unconfirmed. Required input: user explicit approval of current plan_version.
- Unrecoverable closed case status, no execution without a new case or explicit
  reconfirmation.
- TDD required but no observed expected Red, or unexpected Red failure, no
  implementation.
- TDD exception without alternative verification -> wait for user input.
- Serious plan deviation, stop. → Route: plan-confirm. Reason: plan deviation exceeds adaptive scope. Required input: plan amendment.
- Touching non-goals or violating decisions, stop.
- Do not modify authority docs.
- Do not modify scripts, dependencies, lockfiles, CI, or test command config
  unless authorized.
