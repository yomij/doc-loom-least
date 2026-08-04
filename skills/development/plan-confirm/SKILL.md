---
name: plan-confirm
description: Write, version, and authorize a compact or guarded Doc Loom implementation plan after case identity and context or a valid skip exist. Own the outcome envelope, risk, baseline, TDD, conditional review/commit strategy, and approval; never execute an unauthorized plan.
---

# plan-confirm

Consume an existing case; missing identity returns to `docloom-workflow`.

Read when trigger condition is met:

- [Shared protocol](./references/shared-protocol.md): shared status, risk,
  authorization, or commit rules.
- [TDD exceptions](./references/tdd-exceptions.md): planning
  `TDD Required: No` only.
- [Plan template](./templates/plan.md): writing `plan.md` as `status: draft`.

## Inputs

Require the request, case id, Git baseline, context summary/brief or valid
context skip, relevant authority, current plan when revising, and confirmed
decisions/candidate selection.

## Workflow

1. Validate context and case; choose `isolated`, `branch`, or `inline`.
2. If a requested requirements artifact exists, require approval and its
   declared `Doc-Loom-Step: requirements` commit before planning; never commit
   a draft as approved.
3. Classify risk and TDD applicability; decide whether guarded assurance is
   required independently of case persistence.
4. Define the outcome envelope: Goal, Guardrails/Non-goals, Acceptance,
   Escalation Triggers, and any protected boundary. List likely file owners and
   tests as adaptive planning evidence, not approval boundaries.
5. Write small executable tasks and conditional review/commit strategy. For
   guarded work, lead with a Human Approval Summary covering outcome, material
   scope, decision, local Git actions, interruptions, and excluded actions.
6. Write `plan.md` from the plan template as `status: draft`; self-review for
   missing paths, placeholders, coverage, naming, buildability, and TDD
   integrity.
7. For compact persistent work, record a current unambiguous execute request as
   approval; ask only when intent or outcome envelope is ambiguous. For guarded
   work, explicitly confirm the written current plan.
8. On approval, record approver/time/version/baseline and Confirmation Log.
9. Commit approved plan evidence only when user/project intent or the plan's
   guarded audit strategy declares semantic value for that commit.
10. Continue same-turn to execution unless the user holds, revises, or requests
    review first.

## Plan Contract

Frontmatter: `case_id`, `plan_version`, `status`, `risk_level`, `approved_by`,
`approved_at`, and `base_commit` (or Git-unavailable reason).

Core body: Goal, Guardrails/Non-goals, Acceptance, Escalation Triggers, context,
workspace baseline, risk/assurance mode, TDD applicability, likely files/tasks,
and Confirmation Log. Guarded plans also include the Human Approval Summary
and exact-baseline Engineering/Spec review. Define commits only when requested
or semantically valuable, with explicit scope, staging, titles/trailers,
verification, and Git exclusions. Add discovery, decisions, assumptions,
amendments, tests, risks, or docs impact only when material.

The baseline is the exact pre-execution point and never contains a later plan
commit's own hash. Requirements approval authorizes only its declared effect.
Current plan approval authorizes the outcome envelope, not a frozen file list.

## Version And Approval

| Event | Record |
|---|---|
| Draft | `status: draft`; approval empty. |
| User approval | `status: approved`; user/time/version/baseline recorded. |
| Current compact execute request | `status: approved`; record request/time/version/baseline. |
| Escalation-trigger change | Increment version, return to draft, clear current approval. |

Reapproval is required for outcome/non-goal or acceptance changes, risk
escalation, authority/public-contract change, dependency/lockfile/CI/schema/
config-contract effect, external resource or irreversible action, or another
explicitly protected boundary. Internal file discovery, added tests, ordinary
implementation choices, and checkbox/progress edits do not change the approved
version while they stay inside the envelope.

## Gates

- No context/valid skip, identity, or resolvable conflict -> return to owner.
- No approved requirements when required -> no plan execution.
- No current recorded authorization -> no execution.
- High risk requires an unambiguous current object and matching log version.
- Missing guarded Human Approval Summary, required review strategy, or any
  declared plan commit -> correct before execution.
- A discovery candidate is context, never approval.
