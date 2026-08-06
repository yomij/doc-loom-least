---
case_id: 20260806-p1-terminal-status-carrier
plan_version: 1
status: approved
risk_level: high
approved_by: user
approved_at: 2026-08-06T09:49:04+08:00
base_commit: 6b967c499216e22b1d5485b111ebf4a17a815c54
---

# Implementation Plan — P1 terminal status carrier

## Human Approval Summary

- Outcome: Compact persistent cases close without a separate `closure.md`; terminal status lives on `plan.md`. Guarded cases keep a thin `closure.md` as terminal SSOT. Done Gate and residual/follow-up duties stay.
- Material scope: shared protocol, `doc-sync-close`, plan/closure templates, thin doorway/context wording, narrow `development-flow` authority patch, and derived README/dashboard/product-state wording that still claims “every case needs `closure.md`”.
- Decision: user selected product option P1, then said “执行” on that locked design.
- Expected local Git actions: one green implementation commit (skills + authority + case evidence as needed); optional review-fix; this case itself is guarded so it ends with a thin `closure.md`. No push/PR/history rewrite.
- Likely interruptions: none expected if scope stays on terminal-carrier policy only.
- Excluded actions: Campaign mode, changing Direct path, weakening Done Gate, deleting closure as a concept for guarded work, dependency/CI/schema changes, publication.

## Goal

Implement product decision **P1**: terminal-status *function* remains; independent full `closure.md` is not required on Compact; Guarded requires thin `closure.md`.

## Guardrails / Non-goals

- Do not remove final statuses, Done Gate, residuals, or follow-ups.
- Do not merge all terminal state into `execution.md` (that was P2).
- Do not force migration of historical cases that already have `closure.md`.
- Do not add stages, skills, or numeric size thresholds.
- Do not change risk levels or proportional Direct/Compact/Guarded entry questions beyond terminal-carrier rules.

## Context

- Summary: User dogfood showed ceremony cost; prior proportional simplification already added Direct/Compact/Guarded and outcome envelopes. Remaining gap: Compact still requires `closure.md` for every ended case, and templates still invite thick closure reports.
- Route verdict: `proceed_to_plan` (workflow/agent policy; guarded). Context skip: conversation holds locked P1 contract; constitution min-path/human-first applies.
- Decision record: P1 chosen over P2 after explicit comparison.

## Workspace Baseline

- Base commit: `6b967c499216e22b1d5485b111ebf4a17a815c54`
- Branch / run mode: `main` / `inline`
- Dirty or existing changes: none at approval

## Risk Level

- Guarded assurance required: Yes
- Reason: workflow/agent policy and authority narrow patch; reversible docs/skills only, but durable agent behavior changes.

## TDD Applicability

- TDD Required: No
- Reason and alternative verification: contract Markdown only. Verify with exact-path greps for required/forbidden carrier wording, status-order checks, thin-closure template constraints, and paper replay of Compact vs Guarded terminal rules.

## Files to Change

- `skills/_shared/references/shared-protocol.md`
- `skills/development/doc-sync-close/SKILL.md`
- `skills/development/doc-sync-close/templates/closure.md`
- `skills/development/plan-confirm/templates/plan.md`
- `skills/development/docloom-workflow/SKILL.md` (only if status/closure pointers stale)
- `skills/development/context-authority/SKILL.md` (resume read order)
- `docs/authority/workflow/development-flow.md` (confirmed narrow patch)
- `docs/authority/operations/git.md` (if it assumes every case has closure.md hash rules only — keep hash rule, clarify carrier)
- `README.md` (and `README_CN.md` if present and stale)
- `docs/cases/README.md`, `docs/product/current-state.md`
- this case’s plan/execution/thin closure

## Acceptance Criteria

| Criterion | Planned verification |
|---|---|
| Compact ended cases must not require creating `closure.md` | shared-protocol + doc-sync-close gates |
| Compact terminal fields on plan: `final_status` + `closed_at` (and optional Final status body) | plan template + protocol |
| Guarded ended cases require thin `closure.md` | protocol + doc-sync-close + template |
| Thin closure forbids default full tests/review/commit restatement | closure template HTML + skill wording |
| Status derivation: existing `closure.md` first (legacy/Guarded), else plan `final_status` | shared-protocol order |
| Closing only plan terminal fields is not an escalation/reapproval | plan-confirm or protocol note |
| Historical cases with `closure.md` remain valid | compatibility wording |
| Done Gate still blocks unqualified Done without acceptance/review when triggered | doc-sync-close Done Gate preserved |
| Authority development-flow matches skill carrier rules | narrow patch applied |

## Escalation Triggers

- Changed outcome, non-goal, or acceptance:
- Risk / authority / public contract:
- Dependency, lockfile, CI, schema, or config contract:
- External resource or irreversible action:
- Other protected boundary: any move toward P2 (execution-owned terminal only) or deleting Guarded thin closure

## Post-Execution Review Strategy

- Engineering: exact wording consistency across protocol, close owner, templates, doorway/context; no contradictory “every case needs closure.md”.
- Spec: Goal/Acceptance/P1 decision; Compact vs Guarded carrier; thin rules; status order; Done Gate retained.
- Baseline: `6b967c499216e22b1d5485b111ebf4a17a815c54..HEAD` complete explainable target.

## Tasks

### Task 1: Kernel + close owner + templates

- [ ] Update shared-protocol Proportional Path, Artifacts, and status derivation for dual carriers.
- [ ] Update doc-sync-close write target by assurance mode; thin Guarded rules; Compact plan terminal writeback.
- [ ] Update plan and closure templates.

### Task 2: Authority and derived surfaces

- [ ] Narrow-patch development-flow (and git if needed).
- [ ] Align README/dashboard/product-state pointers.
- [ ] Grep active (non-archive) docs for stale “every case needs closure.md”.

### Task 3: Verify, review, close this guarded case

- [ ] Run semantic greps / paper replay.
- [ ] Post-execution Engineering/Spec; fix if needed.
- [ ] Thin `closure.md` for this case; refresh dashboard.

## Confirmation Log

| When | Confirmed By | Plan Version | Confirmation |
|---|---|---:|---|
| 2026-08-06 | user | pre | Selected product option P1 over P2. |
| 2026-08-06T09:49:04+08:00 | user | 1 | “执行” — authorize implementation of the locked P1 terminal-carrier design in this plan. |
