---
case_id: 20260708-plan-approval-direct-execute
plan_version: 1
status: approved
risk_level: high
approved_by: user
approved_at: 2026-07-08T16:47:41+08:00
base_commit: f2d7967d6036f079c89e4f249ea86306ddcee1b4
---

# Implementation Plan

## Goal

Change Doc Loom plan approval semantics so confirming the current plan also
authorizes immediate execution in the same turn by default, unless the user
asks to stop after planning.

The intended behavior is: `Approve plan vN` means "approve this plan and proceed
to execution" when the current plan object is unambiguous and execution
preflight passes.

## Non-goals

- Do not auto-execute a historical approved plan discovered during status,
  resume, or a later unrelated turn without current user intent.
- Do not weaken the requirement that high-risk approval identify the current
  object.
- Do not change Fast-Path conditions.
- Do not auto-trigger `review` or `grill`.
- Do not add a CLI backend, daemon, scheduler, queue, or background worker.
- Do not change staging, commit, push, or PR behavior.

## Context

- Summary / Brief: inline context-authority summary.
- Route Verdict: `proceed_to_plan_with_risk`.
- Skipped Context Reason: none.

Context summary:

- User first asked whether plan approval automatically enters execution; current
  authority and skill rules say ordinary approved plans wait unless the user
  explicitly asks to execute or continue.
- User then stated the system should change to direct execution.
- `docs/authority/constitution.md` says Doc Loom should avoid forcing users to
  perform unnecessary work, but should interrupt for real human decisions or
  explicit authorization.
- `docs/authority/workflow/development-flow.md` currently says
  `plan-confirm` waits for confirmation before persistent execution unless all
  Fast-Path conditions hold, and `tdd-execute` executes only an approved case
  plan.
- Current skill rules place execution authorization in a separate "execute or
  continue" request, except for Fast-Path and same-turn "approve and execute".

Context sources:

| Source | Layer | Why Included | Trust |
|---|---|---|---|
| `docs/authority/constitution.md` | L1 authority | Human-first constraint and authorization boundary. | Highest |
| `docs/authority/workflow/development-flow.md` | Active authority | Current workflow confirmation policy. | High |
| `skills/_shared/references/shared-protocol.md` | Current skill protocol | Cross-skill confirmation semantics, instruction order, high-risk topics. | High |
| `skills/development/docloom-workflow/SKILL.md` | Current skill implementation | Route table and gate for approved plans. | High |
| `skills/development/plan-confirm/SKILL.md` | Current skill implementation | Plan confirmation state transitions. | High |
| `skills/development/tdd-execute/SKILL.md` | Current skill implementation | Execution preflight and same-turn approval behavior. | High |
| `skills/development/plan-confirm/references/risk-levels.md` | Current skill reference | High-risk classification for workflow policy changes. | High |
| User message, 2026-07-08 | Pending fact | Owner preference to change plan approval into direct execution. | Pending until plan approval |

## Decisions

- Treat current-plan approval as execution authorization in the same turn unless
  the user explicitly says plan-only, hold, pause, or review first.
- Preserve the old wait behavior for approved plans found later through status,
  resume, case discovery, or an unrelated conversation turn unless the user
  currently asks to execute or continue.
- Keep high-risk approval strict: the current plan object must be unambiguous.
- Keep `review` and `grill` manual-only.

## Workspace Baseline

- Base Commit: `f2d7967d6036f079c89e4f249ea86306ddcee1b4`
- Dirty Workspace Before Case Creation: no
- Existing Changed Files Before Case Creation: none
- Branch: `main`
- Run Mode: inline

## Risk Level

High.

Reason: this changes workflow and agent policy semantics for when an approved
plan authorizes implementation work.

## TDD Applicability

- TDD Required: No
- If No, Reason: Pure documentation and Markdown skill-rule update. The repo has
  no runtime source tree or automated workflow interpreter test suite for these
  skill semantics.
- Alternative Verification:
  - `git diff --check`
  - grep for stale plan-approval gating wording
  - inspect changed authority and skill docs for consistent same-turn approval,
    stale-plan guard, high-risk confirmation, and manual-only review/grill
    semantics

## Files to Change

Modify:

- `docs/authority/workflow/development-flow.md`
- `skills/_shared/references/shared-protocol.md`
- `skills/development/docloom-workflow/SKILL.md`
- `skills/development/plan-confirm/SKILL.md`
- `skills/development/tdd-execute/SKILL.md`
- `docs/cases/README.md`
- `docs/cases/20260708-plan-approval-direct-execute/case_state.yaml`
- `docs/cases/20260708-plan-approval-direct-execute/plan.md`

Create:

- `docs/cases/20260708-plan-approval-direct-execute/execution.md`
- `docs/cases/20260708-plan-approval-direct-execute/closure.md`

## Acceptance Criteria

| Criteria | Planned Verification |
|---|---|
| Current-turn approval of the current `plan.md` authorizes immediate execution by default. | Inspect `development-flow.md`, `shared-protocol.md`, `docloom-workflow/SKILL.md`, `plan-confirm/SKILL.md`, and `tdd-execute/SKILL.md`. |
| A historical approved plan does not execute merely because it exists. | Inspect route table and gates for status/resume wording. |
| High-risk plans still require explicit approval of an unambiguous current object before execution. | Inspect confirmation semantics in shared protocol, authority, and stage skills. |
| Users can still approve plan-only by saying hold, pause, plan-only, or review first. | Inspect plan-confirm and routing wording. |
| `review` and `grill` remain manual-only. | Inspect `docloom-workflow/SKILL.md` and shared protocol wording. |
| Fast-Path remains limited to verified low-risk current requests and does not become background execution permission. | Inspect Fast-Path wording. |
| No backend, daemon, scheduler, queue, or CLI orchestration is introduced. | Inspect changed files and grep for forbidden orchestration terms if needed. |
| Markdown whitespace is valid. | Run `git diff --check`. |

## Tasks

### Task 1: Update authority confirmation policy

**Files:**
- Modify: `docs/authority/workflow/development-flow.md`

- [x] Replace the separate "confirmation before execution" model with
  current-turn approval-as-execution-authorization.
- [x] Preserve stale approved plan and high-risk confirmation boundaries.

### Task 2: Update shared protocol semantics

**Files:**
- Modify: `skills/_shared/references/shared-protocol.md`

- [x] Update Route Output and Confirmation Semantics so current plan approval
  may continue to execution in the same session.
- [x] State that historical approved plans still require current execute,
  continue, or approval intent.

### Task 3: Update stage skill routing

**Files:**
- Modify: `skills/development/docloom-workflow/SKILL.md`
- Modify: `skills/development/plan-confirm/SKILL.md`
- Modify: `skills/development/tdd-execute/SKILL.md`

- [x] Change `docloom-workflow` routing and gates to route same-turn plan
  approval to `tdd-execute`.
- [x] Change `plan-confirm` post-confirmation behavior to continue unless the
  user asks to stop after planning.
- [x] Change `tdd-execute` preflight to treat current-plan approval as
  execution authorization after approval writeback.

### Task 4: Verify and close

**Files:**
- Create: `execution.md`
- Create: `closure.md`
- Modify: `docs/cases/README.md`
- Modify: `case_state.yaml`
- Modify: `plan.md`

- [x] Run `git diff --check`.
- [x] Run stale wording grep checks.
- [x] Record final evidence and closure.
- [x] Refresh the case dashboard.

## Risks

| Risk | Mitigation |
|---|---|
| "Direct execution" could be misread as background execution of old approved plans. | The plan explicitly scopes the rule to current-turn approval of the current plan object. |
| High-risk approval could become too casual. | Preserve the existing high-risk object-identification requirement. |
| The workflow could make review feel automatic. | Keep `review` and `grill` manual-only in unchanged gates. |

## Documentation Impact

- Updates active workflow authority and current skill implementation.
- Does not change the constitution.
- Does not change product scope, repository architecture, distribution, or git
  authority docs.

## Confirmation Log

| When | Confirmed By | Plan Version | Confirmation |
|---|---|---|---|
| 2026-07-08T16:47:41+08:00 | user | 1 | "可以 批准执行" |
