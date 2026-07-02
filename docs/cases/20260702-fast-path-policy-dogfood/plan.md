---
case_id: 20260702-fast-path-policy-dogfood
plan_version: 1
status: approved
risk_level: high
approved_by: user
approved_at: 2026-07-02T19:22:15+08:00
base_commit: e3f7c2e8d6b445ac41d6a52419a73dd45a53bbc5
---

# Implementation Plan

## Goal

Dogfood a real Doc Loom case by binding the current Fast-Path confirmation-policy
fix to persistent case artifacts, execution evidence, and closure.

## Non-goals

- Do not change the already-written Fast-Path policy diff unless verification
  exposes a concrete defect.
- Do not create a separate governance batch.
- Do not stage, commit, or push.
- Do not auto-trigger `review`.
- Do not reduce closure status values or restructure shared references in this
  case.

## Context

- Summary / Brief: See `context-authority-brief.md`.
- Route Verdict: `proceed_to_plan_with_risk`.
- Skipped Context Reason: none.

This is a recovery dogfood case: the policy patch was already produced in the
current dirty workspace before this case was created. The case intentionally
tests whether Doc Loom can bind real in-progress work to durable evidence and
close it honestly.

## Workspace Baseline

- Base Commit: `e3f7c2e8d6b445ac41d6a52419a73dd45a53bbc5`
- Dirty Workspace: yes
- Existing Changed Files:
  - `docs/authority/README.md`
  - `docs/authority/workflow/development-flow.md`
  - `docs/governance/2026-07-02-full-repo-docs-governance.md`
  - `docs/share/workflow-and-design.md`
  - `skills/_shared/references/shared-protocol.md`
  - `skills/development/doc-sync-close/SKILL.md`
  - `skills/development/docloom-workflow/SKILL.md`
  - `skills/development/plan-confirm/SKILL.md`
  - `skills/development/plan-confirm/references/risk-levels.md`
  - `skills/development/plan-confirm/templates/plan.md`
  - `skills/development/tdd-execute/SKILL.md`

## Risk Level

High. The underlying diff touches workflow/agent policy, shared protocol, stage
skill gates, and authority docs.

## TDD Applicability

- TDD Required: No
- If No, Reason: Pure documentation and Agent Skill rule update; no executable
  runtime or test suite exists in this repository.
- Alternative Verification:
  - `git diff --check`
  - active-doc grep for stale Fast-Path blocker wording
  - symlink integrity check under `skills/`
  - `git status --short`

## Files to Change

Create:
- `docs/cases/20260702-fast-path-policy-dogfood/case_state.yaml`
- `docs/cases/20260702-fast-path-policy-dogfood/context-authority-brief.md`
- `docs/cases/20260702-fast-path-policy-dogfood/plan.md`
- `docs/cases/20260702-fast-path-policy-dogfood/execution.md`
- `docs/cases/20260702-fast-path-policy-dogfood/closure.md`

Modify:
- none for the policy diff unless validation finds an issue.

## Acceptance Criteria

| Criteria | Verification |
|---|---|
| Case identity exists and all required case artifacts are written. | `find docs/cases/20260702-fast-path-policy-dogfood -maxdepth 1 -type f` |
| Plan records the recovery nature of this dogfood case. | Inspect `plan.md` and `execution.md`. |
| Execution evidence captures current dirty diff and verification commands. | Inspect `execution.md`; run listed commands. |
| Closure records dogfood findings and final status. | Inspect `closure.md` and `case_state.yaml`. |
| Active runtime/authority docs no longer contain the old Fast-Path blocker wording. | `rg -n -g '!docs/archive/**' -g '!docs/cases/**' "<stale patterns>" .` |
| Skill symlinks remain valid. | `find skills -type l ...` check emits no output. |

## Tasks

### Task 1: Create recovery dogfood case artifacts

**Files:**
- Create: `docs/cases/20260702-fast-path-policy-dogfood/**`
- Modify: none outside the case
- Test: command/text verification

- [x] **Step 1: Write the plan and context lock**
- [x] **Step 2: Record current diff as execution baseline**
- [x] **Step 3: Write execution evidence**
- [x] **Step 4: Run target verification**
- [x] **Step 5: Write closure and close case state**

## Confirmation Log

| When | Confirmed By | Plan Version | Confirmation |
|---|---|---|---|
| 2026-07-02T19:22:15+08:00 | user | 1 | "接着 用一个真实 case dogfood" after requesting execution of the synthesized improvement plan. |
