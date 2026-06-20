---
name: plan-confirm
description: Create and confirm an implementation plan before execution in a document-driven workflow. Use after docloom-workflow has established case_id and case_docs, and after context-authority when a context gate is needed. Produces plan.md with decisions, risk level, TDD strategy, optional adaptive execution policy, documentation impact, plan_version, base_commit, and explicit user confirmation before tdd-execute.
---

# plan-confirm

Create the execution plan and get user confirmation before `tdd-execute`.
`plan-confirm` consumes case identity; it does not create a case id or case docs.

Read shared rules when needed: `../_shared/references/shared-protocol.md`.

## Required Inputs

- Current user request.
- `case_id` and `docs/cases/<case-id>/` from `docloom-workflow`.
- Context summary or `context-authority-brief.md`.
- Route verdict and context sources.
- Current git baseline.
- Relevant authority docs.
- Existing `plan.md` when revising a plan.
- User-confirmed discussion decisions that affect this plan.

If `case_id` or case docs are missing, stop with
`needs_docloom_workflow` / `needs_case_initialization`.

## Workflow

1. Validate context. No context, no plan.
2. Confirm case id, case docs, and run mode: `isolated`, `branch`, or `inline`.
3. Classify risk: `low`, `medium`, or `high`.
4. Decide TDD applicability.
5. Lock file structure and file responsibilities.
6. Write bite-sized implementation tasks with exact files, commands, expected
   results, and acceptance criteria.
7. Write `docs/cases/<case-id>/plan.md` as `status: draft`.
8. Self-review for coverage, placeholders, name consistency, buildability, and
   TDD integrity.
9. Ask for user confirmation. High risk requires explicit confirmation.
10. After confirmation, update `plan.md`, `case_state.yaml`, and `handoff.md`
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

Plan content must include:

- Goal and non-goals.
- Decisions confirmed by the user.
- Assumptions and context sources.
- Workspace baseline.
- Risk level and reason.
- TDD applicability and strategy or confirmed exception.
- File structure and files to change.
- Tests to add or update.
- Acceptance criteria.
- Documentation impact.
- Confirmation log.

Use `templates/plan.md` as the shape.

## TDD Applicability

Default:

```text
TDD Required: Yes
```

Allowed exceptions include pure documentation, configuration, build script
repair, telemetry, UI copy, dead code deletion, exploratory spike, urgent
hotfix, or environment work that cannot be reliably automated.

Exception format:

```md
## TDD Applicability

- TDD Required: No
- If No, Reason:
- Alternative Verification:
  - manual check
  - snapshot check
  - build check
  - smoke test
  - reviewer confirmation
```

Do not manufacture meaningless failing tests for behavior-preserving refactors.
Use characterization or existing behavior lock, refactor, then verify no
regression.

## Version And Confirmation

`plan_version` is the semantic version of the user-confirmed plan.

On confirmation:

- Record current `plan_version`.
- Set `approved_by: user`.
- Set `approved_at`.
- Record `base_commit` or an unavailable reason.
- Add a Confirmation Log entry.
- Set `case_state.yaml` phase to `planned`.

Material plan changes require:

- Increment `plan_version`.
- Reset `status: draft`.
- Clear approval fields or make them no longer current.
- Ask for confirmation again.

Material changes include goal, decisions, file scope, test strategy, risk level,
authority impact, TDD exception, public contract, or file responsibility
changes. Checkbox/status edits do not change plan semantics.

## writing-plans Boundary

Use `writing-plans` as a quality reference only:

- Exact paths, commands, and expected results.
- No placeholders.
- Clear file responsibilities.
- TDD for behavior changes unless exception is confirmed.

Do not inherit:

- `docs/superpowers/plans/**`.
- Dedicated worktree as a hard prerequisite.
- Subagent-driven vs inline execution choice.
- `superpowers:executing-plans` as the execution entry.

## Gates

- No context, no plan.
- No `case_id` or case docs, no plan.
- Blocking conflict means no execution plan.
- No user confirmation, no `tdd-execute`.
- Current `plan_version` must be approved before execution.
- Discussion decisions that change execution constraints must enter
  `## Decisions` and be confirmed.
