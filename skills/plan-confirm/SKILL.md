---
name: plan-confirm
description: Create and confirm an implementation plan before execution. Use after case identity exists and context is gathered or explicitly skipped.
---

# plan-confirm

`plan-confirm` consumes case identity; it does not create a case id or case docs.

Read when trigger condition is met:

- `references/shared-protocol.md` when: writing case_state.yaml phase update,
  checking artifact policy, or checking execution instruction order.
- `references/risk-levels.md` when: classifying risk level, writing
  plan_confirmation_policy, or deciding TDD applicability exception based on
  risk.

## Required Inputs

- Current user request.
- `case_id` and `docs/cases/<case-id>/` from `docloom-workflow`.
- Context summary, `context-authority-brief.md`, or explicit low-risk
  skipped-context reason.
- Route verdict and context sources when `context-authority` ran.
- Current git baseline.
- Relevant authority docs.
- Existing `plan.md` when revising a plan.
- User-confirmed discussion decisions that affect this plan.

If `case_id` or case docs are missing, stop with
`needs_docloom_workflow` / `needs_case_initialization`.

## Workflow

1. Validate context or the explicit low-risk skipped-context reason. No context
   and no skip reason means no plan.
2. Confirm case id, case docs, and run mode: `isolated`, `branch`, or `inline`.
3. Classify risk: `low`, `medium`, or `high`.
4. Decide TDD applicability.
5. Lock file structure and file responsibilities.
6. Write bite-sized implementation tasks with exact files, commands, expected
   results, and acceptance criteria.
7. Write `docs/cases/<case-id>/plan.md` as `status: draft`.
8. Update `case_state.yaml` to `phase: waiting_for_plan_confirmation` with the
   current `plan_version`.
9. Self-review for coverage, placeholders, name consistency, buildability, and
   TDD integrity.
10. Ask for user confirmation. High risk requires explicit confirmation tied
    to the current `plan_version`.
11. After confirmation, update `plan.md`, `case_state.yaml`, and `handoff.md`
    when a future resume point exists. Use `templates/handoff.md`.

## Plan Rules

`plan.md` frontmatter:

```yaml
---
case_id:
plan_version: 1
status: draft
risk_level:
approved_by:
approved_at:
base_commit:
---
```

Plan core content must include:

- Goal and non-goals.
- Context summary, context brief link, or skipped-context reason.
- Workspace baseline.
- Risk level and reason.
- TDD applicability and strategy or confirmed exception.
- Files to change.
- Acceptance criteria.
- Tasks with exact paths, commands, expected results, and no placeholders.
- Confirmation log.

Include triggered sections only when they have content: confirmed decisions,
non-obvious assumptions, detailed TDD plan, adaptive execution, plan amendments,
tests to add or update, risks, or documentation impact.

Use `templates/plan.md` as the shape.

## TDD Applicability

Default: TDD Required: Yes.

Allowed exception categories are defined in `tdd-execute/references/tdd-exceptions.md`.
Plan must record: TDD Required (Yes/No), Reason if No, Alternative Verification. When
declaring TDD Required: No, cite the specific category from tdd-exceptions.md.

Do not manufacture meaningless failing tests for behavior-preserving refactors.
Use characterization or existing behavior lock, refactor, then verify no
regression.

## Version And Confirmation

`plan_version` is the semantic version of the user-confirmed plan.

| Event | `plan.md` status | `case_state.yaml` phase | Approval fields |
|---|---|---|---|
| Draft written | `draft` | `waiting_for_plan_confirmation` | Empty |
| User confirms current version | `approved` | `planned` | `approved_by: user`, `approved_at`, `base_commit` |
| Material change | `draft` | `waiting_for_plan_confirmation` | Cleared or no longer current |

On confirmation, add a Confirmation Log entry and record `base_commit` or an
unavailable reason. When `git_available: false`, leave `base_commit` empty and
record the reason; this does not block plan creation.

Material plan changes require incrementing `plan_version` and asking for
confirmation again.

Material changes include goal, decisions, file scope, test strategy, risk level,
authority impact, TDD exception, public contract, or file responsibility
changes. Checkbox/status edits do not change plan semantics.

## Gates

- No context and no skipped-context reason, no plan. → Route: context-authority. Reason: missing context. Required input: user request and workspace state.
- No `case_id` or case docs, no plan. → Route: docloom-workflow. Reason: missing case identity. Required input: user request for case initialization.
- Blocking conflict means no execution plan. → Route: context-authority. Reason: blocking conflict. Required input: conflict details for resolution.
- No user confirmation, no `tdd-execute` -> wait for user input.
- High-risk confirmation must identify the current `plan_version`; ambiguous
  short confirmation is not enough.
- Current `plan_version` must be approved before execution.
- Discussion decisions that change execution constraints must enter
  `## Decisions` and be confirmed.
