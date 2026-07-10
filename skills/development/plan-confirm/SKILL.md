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
- Selected discovery candidate, when work was triggered by case candidates or
  next-slice discovery.

If `case_id` or case docs are missing, stop with
`needs_docloom_workflow` / `needs_case_initialization`.

## Workflow

1. Validate context or the explicit low-risk skipped-context reason. No context
   and no skip reason means no plan.
2. Confirm case id, case docs, and run mode: `isolated`, `branch`, or `inline`.
3. Classify risk: `low`, `medium`, or `high`.
4. Decide TDD applicability.
5. Decide whether the case is eligible for mandatory Post-execution review and
   semantic atomic commits; record compact strategies when it is.
6. Lock file structure and file responsibilities.
7. Write bite-sized implementation tasks with exact files, commands, expected
   results, and acceptance criteria.
8. Write `docs/cases/<case-id>/plan.md` as `status: draft`.
9. Update `case_state.yaml` to `phase: waiting_for_plan_confirmation` with the
   current `plan_version`.
10. Self-review for coverage, placeholders, name consistency, buildability, and
   TDD integrity.
11. Ask for user confirmation unless Fast-Path conditions are verified. High
    risk requires explicit confirmation of the current plan object, but the
    user's wording does not have to include `plan_version` when the object is
    unambiguous.
12. After confirmation or Fast-Path approval, update `plan.md`,
    `case_state.yaml`, and `handoff.md` when a future resume point exists. Use
    `templates/handoff.md`.
13. When the approved Atomic Commit Strategy requires a plan commit and Git is
    available, stage only the approved plan, routing state, and directly
    related case artifacts; inspect and commit that unit before implementation.
14. On current-plan confirmation, continue to `tdd-execute` same-turn when
    preflight inputs and the required plan commit are available, unless the
    user asks to hold.

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
- Acceptance criteria with planned verification, not final command evidence.
- Tasks with exact paths, planned commands, expected results, and no
  placeholders.
- Confirmation log.
- For eligible cases, `## Post-Execution Review Strategy` and `## Atomic Commit
  Strategy` with baseline, axes, gate/fix behavior, semantic boundaries,
  staging scope, titles/trailers, and excluded Git actions.

When a discovery candidate triggered the plan, include `## Discovery Candidate`
with: Mode, Candidate ID, Evidence, Recommendation Reason, and User Decision.

Include triggered sections only when they have content: confirmed decisions,
non-obvious assumptions, detailed TDD plan, adaptive execution, plan amendments,
tests to add or update, review strategy, commit strategy, risks, or
documentation impact.

Use `templates/plan.md` as the shape.

## TDD Applicability

Default: TDD Required: Yes.

Allowed exception categories are defined in `../tdd-execute/references/tdd-exceptions.md`.
Plan must record: TDD Required (Yes/No), Reason if No, Alternative Verification. When
declaring TDD Required: No, cite the specific category from tdd-exceptions.md.

For behavior-preserving refactors, see `../tdd-execute/references/tdd-exceptions.md`
for the characterization workflow.

## Version And Confirmation

`plan_version` is the semantic version of the approved plan. It is an internal
traceability field; user approval wording does not have to name it when the
approved plan is unambiguous.

| Event | `plan.md` status | `case_state.yaml` phase | Approval fields |
|---|---|---|---|
| Draft written | `draft` | `waiting_for_plan_confirmation` | Empty |
| User confirms current plan | `approved` | `planned`, then same-turn `tdd-execute` unless the user asks to hold | `approved_by: user`, `approved_at`, `base_commit` |
| Fast-path conditions verified | `approved` | `planned` | `approved_by: fast-path`, `approved_at`, `base_commit` or unavailable reason |
| Material change | `draft` | `waiting_for_plan_confirmation` | Cleared or no longer current |

On user confirmation or fast-path approval, add a Confirmation Log entry that
records the current `plan_version`, then record `base_commit` or an unavailable
reason. When `git_available: false`, leave `base_commit` empty and record the
reason; this does not block plan creation.

Current-plan approval authorizes the case-scoped commits declared by the
Atomic Commit Strategy without repeated confirmation. It does not authorize
unrelated files, push, PR, merge, tag, release, amend, rebase, squash, history
rewriting, material plan deviations, or unlisted dependency, lockfile, CI,
schema, config-contract, or authority changes.

If an approved requirements artifact exists, verify its approval commit before
creating the plan commit. The plan's `base_commit` remains the exact
pre-execution baseline; do not try to embed the hash of the commit containing
the plan itself.

Material plan changes require incrementing `plan_version` and asking for
confirmation again.

Material changes include goal, decisions, file scope, test strategy, risk level,
authority impact, TDD exception, public contract, or file responsibility
changes. Checkbox/status edits do not change plan semantics.

## Gates

- No context and no skipped-context reason, no plan. → Route: context-authority. Reason: missing context. Required input: user request and workspace state.
- No `case_id` or case docs, no plan. → Route: docloom-workflow. Reason: missing case identity. Required input: user request for case initialization.
- Blocking conflict means no execution plan. → Route: context-authority. Reason: blocking conflict. Required input: conflict details for resolution.
- No user confirmation or valid fast-path approval, no `tdd-execute` -> wait for user input.
- Current-plan confirmation -> same-turn `tdd-execute` unless plan-only, hold,
  revise, or review.
- Eligible plan with Git available but no required atomic plan commit -> do not
  begin implementation.
- Later-discovered approved plan -> needs current execute, continue, or
  reconfirm intent.
- High-risk confirmation must identify the current plan object. Naming
  `plan_version` is preferred but not required when the object is unambiguous;
  ambiguous short confirmation is not enough.
- The current plan must be approved and the Confirmation Log must record the
  current `plan_version` before execution. Fast-path plans must record
  `approved_by: fast-path` and the verified Fast-Path conditions.
- Discussion decisions that change execution constraints must enter
  `## Decisions` and be confirmed.
- Eligible plan without Post-Execution Review and Atomic Commit strategies ->
  no execution until the current plan is corrected and confirmed.
- A selected discovery candidate is planning context only. It does not approve
  the plan.
