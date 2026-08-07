---
name: plan-confirm
description: Write, version, and authorize a compact or guarded Doc Loom outcome contract after case identity and context or a valid skip exist. Own Goal, Success Criteria, Constraints, risk/assurance metadata, baseline, and approval; leave implementation paths to execution.
---

# plan-confirm

Consume an existing case; missing identity returns to `docloom-workflow`.

Read when trigger condition is met:

- [Shared protocol](./references/shared-protocol.md): shared status, risk,
  authorization, or commit rules.
- [TDD exceptions](./references/tdd-exceptions.md): validating a proposed
  TDD-exception Constraint only.
- [Plan template](./templates/plan.md): writing `plan.md` as `status: draft`.

## Inputs

Require the request, case id, Git baseline, context summary/brief or valid
context skip, relevant authority, current plan when revising, and confirmed
decisions/candidate selection.

## Workflow

1. Validate context and case; record the exact Git baseline.
2. If a requested requirements artifact exists, require approval and its
   declared `Doc-Loom-Step: requirements` commit before planning; never commit
   a draft as approved.
3. Classify risk and assurance independently of case persistence; keep them as
   frontmatter metadata, not plan-body concerns.
4. Write the outcome contract: Goal, observable Success Criteria with required
   evidence, and Constraints covering guardrails, non-goals, protected effects,
   reapproval triggers, excluded actions, and any owner-mandated restriction.
5. Leave file discovery, task decomposition, sequencing, commands, run mode,
   TDD/verification choice, review invocation, and commit organization to the
   executing agent. A confirmed TDD exception is a Constraint, not a plan.
   For guarded work, summarize outcome, material scope, decision, likely local
   Git effects, interruptions, and exclusions in the confirmation conversation.
6. Write `plan.md` from the plan template as `status: draft`; self-review for
   vague outcomes, non-observable criteria, unsupported completion claims,
   missing constraints, placeholders, and implementation-path leakage.
7. For compact persistent work, record a current unambiguous execute request as
   approval; ask only when intent or the outcome contract is ambiguous. For guarded
   work, explicitly confirm the written current plan.
8. On approval, record approver/time/version/baseline and confirmation metadata.
9. Continue same-turn to execution unless the user holds, revises, or requests
   review first.

## Plan Contract

Frontmatter is concise lifecycle metadata: `case_id`, `plan_version`, `status`,
`risk_level`, `assurance_mode`, `approved_by`, `approved_at`, `confirmation`,
`base_commit` (or Git-unavailable reason), `final_status`, and `closed_at`.

The human-facing body contains exactly three top-level sections:

- **Goal:** the desired end condition and purpose, without a prescribed
  solution shape.
- **Success Criteria:** falsifiable completion claims with the evidence needed
  to support each claim, plus status/evidence columns that closure can update.
- **Constraints:** guardrails, non-goals, protected effects, stop/reconfirm
  conditions, excluded actions, and owner-mandated restrictions.

Do not persist context narratives, assumptions, decisions, files, tasks,
commands, sequencing, run mode, TDD strategy, review strategy, commit strategy,
or a Human Approval Summary in `plan.md`. Context belongs in the inline verdict
or a triggered brief; implementation expectations and actual evidence belong to
execution. Richness comes from precise completion semantics, not more headings.

The baseline is the exact pre-execution point and never contains a later plan
commit's own hash. Requirements approval authorizes only its declared effect.
Current plan approval authorizes Goal, Success Criteria, and Constraints, not an
implementation path. Compact closure may update only criterion status/evidence
and terminal metadata without changing plan semantics.

## Version And Approval

| Event | Record |
|---|---|
| Draft | `status: draft`; approval empty. |
| User approval | `status: approved`; user/time/version/baseline recorded. |
| Current compact execute request | `status: approved`; record request/time/version/baseline. |
| Goal, criterion, or Constraint change | Increment version, return to draft, clear current approval. |

Reapproval is required for Goal, Success Criterion, or Constraint semantics;
risk escalation; authority/public-contract change; dependency/lockfile/CI/
schema/config-contract effect; external resource or irreversible action; or
another protected effect. Criterion status/evidence, internal file discovery,
added tests, and ordinary implementation choices do not change the approved
version while they stay inside the contract.

## Gates

- No context/valid skip, identity, or resolvable conflict -> return to owner.
- No approved requirements when required -> no plan execution.
- No current recorded authorization -> no execution.
- High risk requires an unambiguous current object and matching confirmation/version.
- Missing guarded conversational approval summary, assurance metadata, or exact
  review baseline -> correct before execution.
- A discovery candidate is context, never approval.
