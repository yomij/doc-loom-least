---
name: plan-confirm
description: Write, version, and confirm a Doc Loom implementation plan after case identity and context or a valid skip exist. Own risk, baseline, TDD, review/commit strategy, and approval; never execute an unapproved plan.
---

# plan-confirm

Consume an existing case; missing identity returns to `docloom-workflow`.
Read `references/shared-protocol.md` for shared status/authorization/commit
rules and `references/risk-levels.md` for risk classification. Read
`references/tdd-exceptions.md` only when planning `TDD Required: No`.

## Inputs

Require the request, case id, Git baseline, context summary/brief or valid
low-risk skip, relevant authority, current plan when revising, and confirmed
decisions/candidate selection.

## Workflow

1. Validate context and case; choose `isolated`, `branch`, or `inline`.
2. If a requested requirements artifact exists, require approval and its
   declared `Doc-Loom-Step: requirements` commit before planning; never commit
   a draft as approved.
3. Classify risk, TDD applicability, Fast-Path/full flow, and exact file owners.
4. Write small executable tasks, acceptance with planned verification, and
   review/commit strategies when eligible.
5. Lead with a Human Approval Summary: outcome, material scope, decision,
   local Git actions/commit count, interruptions, and excluded actions.
6. Write `plan.md` from `templates/plan.md` as `status: draft`; self-review for
   missing paths, placeholders, coverage, naming, buildability, and TDD integrity.
7. Ask for confirmation unless every Fast-Path condition is verified.
8. On approval, record approver/time/version/baseline and Confirmation Log;
   Fast-Path also records evidence for every verified condition.
9. For full flow, commit the approved plan/case evidence before implementation;
   Fast-Path keeps its minimal plan for the combined completion unit.
10. Continue same-turn to execution unless the user holds, revises, or requests
    review first.

## Plan Contract

Frontmatter: `case_id`, `plan_version`, `status`, `risk_level`, `approved_by`,
`approved_at`, and `base_commit` (or Git-unavailable reason).

Core body: Human Approval Summary, goal/non-goals, context, workspace baseline,
risk, TDD applicability, exact files/tasks, acceptance, and Confirmation Log.
Eligible plans also define exact-baseline Engineering/Spec review and semantic
atomic commits with explicit staging, titles/trailers, verification, and Git
exclusions. Add discovery, decisions, assumptions, amendments, tests, risks, or
docs impact only when material.

The baseline is the exact pre-execution point and never contains the plan
commit's own hash. Requirements approval authorizes only its commit. Current
plan approval authorizes only the shared/declared case scope.

## Version And Approval

| Event | Record |
|---|---|
| Draft | `status: draft`; approval empty. |
| User approval | `status: approved`; user/time/version/baseline recorded. |
| Verified Fast-Path | `status: approved`; `approved_by: fast-path` plus conditions. |
| Material change | Increment version, return to draft, clear current approval. |

Material changes include goal, decisions, file responsibility, acceptance,
TDD/test strategy, risk, authority/public contract, dependencies/config, or
execution constraints. Checkbox/progress edits are not semantic.

## Gates

- No context/valid skip, identity, or resolvable conflict -> return to owner.
- No approved requirements when required -> no plan execution.
- No current user/Fast-Path approval -> no execution.
- High risk requires an unambiguous current object and matching log version.
- Missing Human Approval Summary, eligible review strategy, commit strategy, or
  required plan commit -> correct before execution.
- A discovery candidate is context, never approval.
