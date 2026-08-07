---
name: plan-confirm
description: Write, version, and authorize a Compact or Guarded outcome contract after case identity and context/valid skip. Own contract, risk/assurance, baseline, and approval; execution owns the path.
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

Require request, case id, Git baseline, context/valid skip, relevant authority,
confirmed selection/decisions, and the current plan when revising.

## Workflow

1. Validate case/context; record the exact baseline and risk/assurance metadata.
2. A requested requirements artifact must be approved and committed with
   `Doc-Loom-Step: requirements` before planning; never commit a draft as approved.
3. Write Goal, evidence-backed Success Criteria, and Constraints. Keep the
   shared adaptive path out; a confirmed TDD exception is a Constraint.
4. For Guarded work, summarize outcome, material scope, decision, local Git,
   interruptions, and exclusions in conversation.
5. Write template `plan.md` as `draft`; reject vague goals, unfalsifiable or
   unsupported criteria, missing constraints, placeholders, and path leakage.
6. Compact may record a current unambiguous execute request; Guarded explicitly
   confirms the written plan. Ask only when intent/contract is ambiguous.
7. Record approver/time/version/baseline/confirmation, then execute same-turn
   unless the user holds, revises, or requests review first.

## Plan Contract

Frontmatter: `case_id`, `plan_version`, `status`, `risk_level`,
`assurance_mode`, `approved_by`, `approved_at`, `confirmation`, `base_commit`
(or Git-unavailable reason), `final_status`, `closed_at`.

The body has exactly:

- **Goal:** desired end state and purpose, never solution shape.
- **Success Criteria:** falsifiable claims, required evidence, and close-time
  status/evidence columns.
- **Constraints:** guardrails, non-goals, protected/reconfirm effects,
  exclusions, and owner mandates.

Persist no other body concern. Context stays inline or in a triggered brief;
execution owns path and evidence. Precision belongs in completion semantics,
not extra headings.

Baseline is the exact pre-execution point, never a later plan commit. Requirements
approval covers only its declared effect. Plan approval covers the contract, not
path. Compact close may update criterion evidence and terminal metadata.

## Version And Approval

| Event | Record |
|---|---|
| Draft | `status: draft`; approval empty. |
| User approval | `status: approved`; user/time/version/baseline recorded. |
| Current compact execute request | `status: approved`; record request/time/version/baseline. |
| Goal, criterion, or Constraint change | Increment version, return to draft, clear current approval. |

Any contract-semantic or shared protected change requires a new draft/version.
Criterion evidence and adaptive-path choices do not.

## Gates

- No context/valid skip, identity, or resolvable conflict -> return to owner.
- No approved requirements when required -> no plan execution.
- No current recorded authorization -> no execution.
- High risk requires an unambiguous object and matching confirmation/version.
- Missing guarded conversational approval summary, assurance metadata, or exact
  review baseline -> correct before execution.
- A discovery candidate is context, never approval.
