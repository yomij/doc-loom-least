---
name: tdd-execute
description: Execute an approved persistent Doc Loom case plan using TDD discipline. Use only after plan-confirm has produced an approved plan with plan_version, risk_level, and base_commit, or after a minimal approval writeback in the same turn. Handles red-green-refactor, TDD exceptions, adaptive execution amendments, implementation, quality checks, execution evidence, handoff, case_state.yaml updates, deviations, review_risk signals, and authorized atomic commits.
---

# tdd-execute

Execute an approved persistent Doc Loom case plan. Ordinary one-shot coding
tasks and simple document edits do not automatically enter this skill.

Read shared rules when needed: `../_shared/references/shared-protocol.md`.
Read TDD exception guidance when needed: `references/tdd-exceptions.md`.

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
2. Red: write or identify the failing test, run the minimal command, and verify
   the failure reason is expected.
3. Green: write minimal implementation and run the target test.
4. Refactor: only planned or necessary refactors; run related tests after each.
5. Quality check with planned or obvious low-risk read-only commands.
6. Update execution docs and plan checkbox progress.
7. Update handoff only when a future resume point exists.
8. Record `review_risk` as a signal only.
9. Check every acceptance criterion.
10. Stage or commit only when the plan or user explicitly authorizes it.

## TDD Rules

Default: write the failing test before implementation.

Do not claim TDD completion unless you observed the test fail for the expected
reason and later pass. If a new test passes on the first run, stop and decide
whether the test is wrong or the behavior already exists.

Allowed Red sources:

- A newly written failing test.
- An existing failing test that directly maps to current acceptance criteria and
  is not unrelated baseline or flaky failure.

Avoid testing anti-patterns:

- Do not assert that a mock exists as proof of behavior.
- Do not add production test-only methods.
- Do not mock dependencies without understanding side effects.
- Mock responses must match real structures.
- Prefer realistic integration tests when mocks exceed the behavior under test.

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

Use `low`, `medium`, or `high`. High risk can come from public API, security,
auth, privacy, billing, data model/migration, insufficient coverage, authority
proposal, or serious plan deviation.

Do not trigger `review`; only the user can request it.

## Commits

Default: do not stage or commit.

Only stage or commit when the plan or user explicitly asks. Stage only
case-related files, preserve unrelated user changes, and record commit hashes
in `execution.md`.

## Gates

- No approved plan, no execution.
- Unconfirmed current `plan_version`, no execution.
- TDD required but no observed expected Red, no implementation.
- Unexpected Red failure, no implementation.
- TDD exception without alternative verification, no execution.
- Serious plan deviation, stop.
- Touching non-goals or violating decisions, stop.
- Do not modify authority docs.
- Do not modify scripts, dependencies, lockfiles, CI, or test command config
  unless authorized.
