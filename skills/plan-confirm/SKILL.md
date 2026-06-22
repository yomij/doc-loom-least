---
name: plan-confirm
description: Create and confirm an implementation plan before execution in a document-driven workflow. Use after docloom-workflow has established case_id and case_docs, and after context-authority when a context gate is needed. Produces plan.md with decisions, risk level, TDD strategy, optional adaptive execution policy, documentation impact, plan_version, base_commit, and explicit user confirmation before tdd-execute.
---

# plan-confirm

Create the execution plan and get user confirmation before `tdd-execute`.
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
    when a future resume point exists.

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
- Tasks with exact paths, commands, and expected results.
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

On draft write:

- Set `case_state.yaml` phase to `waiting_for_plan_confirmation`.
- Record the draft `plan_version`.
- Keep `closure_status: open`.
- Do not set approval fields.

On confirmation:

- Record current `plan_version`.
- Set `approved_by: user`.
- Set `approved_at`.
- Record `base_commit` or an unavailable reason (when `git_available: false`,
  leave `base_commit` empty and record `git_available: false` as the reason;
  this is acceptable and does not block plan creation).
- Add a Confirmation Log entry.
- Set `case_state.yaml` phase to `planned` and `current_plan_version` to the
  approved version.

Material plan changes require:

- Increment `plan_version`.
- Reset `status: draft`.
- Clear approval fields or make them no longer current.
- Ask for confirmation again.

Material changes include goal, decisions, file scope, test strategy, risk level,
authority impact, TDD exception, public contract, or file responsibility
changes. Checkbox/status edits do not change plan semantics.

## Plan Quality Standard

Plans must include: exact paths, commands, and expected results. No placeholders.
Clear file responsibilities. TDD for behavior changes unless exception is confirmed.

## Gates

- No context and no skipped-context reason, no plan. → Route: context-authority. Reason: missing context. Required input: user request and workspace state.
- No `case_id` or case docs, no plan. → Route: docloom-workflow. Reason: missing case identity. Required input: user request for case initialization.
- Blocking conflict means no execution plan. → Route: context-authority. Reason: blocking conflict. Required input: conflict details for resolution.
- Draft plans awaiting user approval must set `case_state.yaml` phase to
  `waiting_for_plan_confirmation`.
- No user confirmation, no `tdd-execute`. → Route: plan-confirm. Reason: plan requires confirmation. Required input: user explicit approval of current plan_version.
- High-risk confirmation must identify the current `plan_version`; ambiguous
  short confirmation is not enough.
- Current `plan_version` must be approved before execution.
- Discussion decisions that change execution constraints must enter
  `## Decisions` and be confirmed.
