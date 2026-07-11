---
name: plan-confirm
description: Create and confirm an implementation plan before execution. Use after case identity exists and context is gathered or explicitly skipped.
---

# plan-confirm

Consume existing case identity; never create a case id or case docs.

Read `references/shared-protocol.md` for state, artifacts, instruction order,
authorization, and Atomic Commit rules. Read `references/risk-levels.md` for
risk, confirmation policy, and risk-based TDD decisions.

## Inputs

- User request, existing case id/docs, and current Git baseline.
- Context summary/brief or explicit low-risk skipped-context reason; include the
  route verdict/sources when `context-authority` ran.
- Relevant authority and existing plan when revising.
- User-confirmed decisions and any selected discovery candidate.

Missing case identity routes to `docloom-workflow` with
`needs_case_initialization`.

## Workflow

1. Validate context or the explicit low-risk skip; no context/skip means no plan.
2. Confirm case id/docs and `isolated`, `branch`, or `inline` run mode.
3. If an explicitly requested requirements artifact exists, verify approval.
   Never commit a draft as approved. When approved, Git is available, and its
   approval commit is missing, stage only requirements plus related routing
   state, inspect, and commit with `Doc-Loom-Step: requirements` before planning;
   in Git degraded mode record why it is absent.
4. Classify `low`/`medium`/`high` risk and decide TDD applicability.
5. Determine whether this is Fast-Path or full eligible work. Both record review
   and commit strategies; Fast-Path declares one combined completion commit and
   no separate plan commit.
6. Lock file responsibilities and write small tasks with exact paths, commands,
   expected results, and acceptance criteria.
7. Lead with a short Human Approval Summary covering outcome, material scope,
   expected local Git actions/commit count, likely interruptions, and excluded
   actions. Then write `plan.md` as `status: draft`; set state to
   `waiting_for_plan_confirmation` with current `plan_version`.
8. Self-review coverage, placeholders, naming, buildability, and TDD integrity.
9. Ask for confirmation unless Fast-Path conditions are verified. High risk
   requires an unambiguous current plan object; version wording is preferred,
   not mandatory.
10. On user/Fast-Path approval, update plan, state, and handoff only when a
    future resume point exists (`templates/handoff.md`).
11. If the approved strategy requires a plan commit and Git is available, stage
    only the approved plan, routing state, and related case artifacts; inspect
    and commit before implementation.
12. Continue same-turn to `tdd-execute` when current-plan approval, preflight
    inputs, and the declared plan-commit condition are satisfied, unless the
    user asks to hold.

## Plan Contract

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

Core content:

- Human Approval Summary; goal/non-goals; context summary/brief/skip; workspace
  baseline; risk/reason.
- TDD applicability and strategy or confirmed exception.
- Files and exact, placeholder-free tasks.
- Acceptance criteria with planned verification, not final command evidence.
- Confirmation Log.
- For eligible cases, `## Post-Execution Review Strategy` and `## Atomic Commit
  Strategy`: baseline, axes, gate/fix behavior, semantic boundaries, staging,
  titles/trailers, and excluded Git actions.

The Human Approval Summary is the confirmation surface, not an extra artifact.
Keep it short enough to read before the detailed plan. State the expected number
and purpose of local commits; say explicitly when push/PR/history rewriting are
excluded. For Fast-Path, describe the single combined completion commit.

When discovery triggered work, add `## Discovery Candidate` with Mode, Candidate
ID, Evidence, Recommendation Reason, and User Decision. Add confirmed decisions,
assumptions, detailed TDD, adaptive execution/amendments, tests, strategies,
risks, or docs impact only when they contain useful information. Use
`templates/plan.md`.

## TDD Applicability

Default: `TDD Required: Yes`. Exceptions and characterization rules live in
`../tdd-execute/references/tdd-exceptions.md`. A `No` plan records its named
category, reason, and Alternative Verification.

## Version And Confirmation

`plan_version` traces plan semantics; approval need not name it when the current
plan is unambiguous.

| Event | Plan/state | Approval |
|---|---|---|
| Draft | `draft` / `waiting_for_plan_confirmation` | Empty |
| User confirms current plan | `approved` / `planned`, then same-turn execution unless held | `approved_by: user`, `approved_at`, `base_commit` |
| Fast-Path verified | `approved` / `planned` | `approved_by: fast-path`, `approved_at`, `base_commit` or unavailable reason |
| Material change | `draft` / `waiting_for_plan_confirmation` | Cleared/not current |

Approval records the current version in Confirmation Log and records
`base_commit` or an unavailable reason. With `git_available: false`, leave the
baseline empty and record why; planning may continue.

Current-plan approval authorizes only commits declared by its Atomic Commit
Strategy; use shared-protocol for the full exclusion list. Requirements approval
authorizes only its requirements commit, not planning/implementation. The plan
baseline remains the exact pre-execution point and never attempts to contain the
hash of its own plan commit.

Fast-Path has no required plan commit. Its approved minimal plan remains in the
working tree until the green change, compact review, closure, and state are
committed together as the declared completion unit.

Goal, decisions, file/responsibility scope, tests/TDD exception, risk, authority
impact, public contract, or other execution-constraint changes are material:
increment the version and reconfirm. Checkbox/status edits are not semantic.

## Gates

- No context/skip -> `context-authority`; no case identity ->
  `docloom-workflow`; blocking conflict -> `context-authority`.
- Unapproved requirements, or approved requirements that cannot be safely
  committed while Git is available -> no implementation plan.
- No user/Fast-Path approval -> no execution. Current approval executes
  same-turn unless plan-only, hold, revise, or review; older approval needs
  current execute/continue/reconfirm intent.
- Full eligible Git plan missing its required plan commit -> no implementation;
  Fast-Path proceeds under its declared combined completion strategy.
- Approval must identify the current object and Confirmation Log version;
  Fast-Path must record its approver and verified conditions.
- Do not ask for confirmation until the Human Approval Summary makes local Git
  effects and excluded publication/history actions obvious.
- Execution-constraint decisions belong in confirmed `## Decisions`.
- Eligible plan missing either review or commit strategy must be corrected and
  reconfirmed before execution.
- A discovery candidate is context, never plan approval.
