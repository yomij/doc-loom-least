---
case_id: 20260703-next-slice-rubric-dogfood
plan_version: 2
status: approved
risk_level: high
approved_by: user
approved_at: 2026-07-03T13:48:54+08:00
base_commit: 77055555fe9ea8a7c4b106e23126368f916cb850
---

# Implementation Plan

## Goal

Compress the default `Next Slice Candidates` output so a human can choose the
next slice from a short decision view, while preserving the full scoring rubric
as internal ranking evidence that can be expanded on request or recorded in
case evidence.

## Non-goals

- Do not change the scoring formula.
- Do not add new workflow stages, artifacts, lifecycle domains, automation, a
  CLI backend, or scheduler behavior.
- Do not change authority docs.
- Do not auto-approve discovered candidates for execution.

## Discovery Candidate

| Field | Value |
|---|---|
| Mode | next-slice discovery dogfood |
| Candidate ID | N1 |
| Evidence | The previous N1 output was actionable but too verbose: the default table exposed all score factors and repeated route/status prose. |
| Recommendation Reason | A compact default output fixes the immediate human-experience issue without changing workflow semantics. |
| User Decision | User asked to compress the output and then confirmed the concrete modification proposal with `改` on 2026-07-03. |

## Decisions

| Decision | Reason |
|---|---|
| Default next-slice output uses a compact 5-column decision table. | The user needs to choose a slice, not audit every scoring factor by default. |
| `Impact / Confidence / Unlock / Effort / Risk` remain internal scoring factors. | Sorting evidence is still useful, but it should be expanded only on request or in case evidence. |
| The decision prompt is shortened to `Decision:`. | This preserves the safety gate with less prose. |

## Context

- Summary / Brief: Inline `context-authority` check. Intent is workflow / skill
  design. Active authority permits next-slice discovery as read-only
  recommendation output; selected candidates still route through
  `plan-confirm` before execution. No authority conflict was found.
- Route Verdict: `proceed_to_plan`.
- Skipped Context Reason: Not skipped; this touches workflow output contract.

```yaml
context_sources:
  case:
    existing_id: 20260703-next-slice-rubric-dogfood
    intent: workflow / skill design
    risk: high
  included:
    - path: docs/authority/constitution.md
      layer: authority
      why_included: minimum path and no complex pipeline constraints
      trust: highest
    - path: docs/authority/README.md
      layer: authority
      why_included: authority order and confirmation policy note
      trust: high
    - path: docs/authority/workflow/development-flow.md
      layer: authority
      why_included: next-slice routing and normal plan-confirm gate
      trust: high
    - path: skills/_shared/references/loop-protocol.md
      layer: skill implementation guidance
      why_included: owns next-slice output shape and scoring rubric
      trust: high
    - path: skills/development/docloom-workflow/SKILL.md
      layer: skill implementation
      why_included: owns docloom-workflow response contract
      trust: high
    - path: docs/product/current-state.md
      layer: operational input
      why_included: explains current bottleneck and dogfood signal
      trust: medium
    - path: docs/cases/README.md
      layer: derived dashboard
      why_included: links the dogfood follow-up
      trust: medium
  excluded:
    - path: docs/archive/**
      reason: archive is historical evidence, not current authority
    - path: package manifests and tests
      reason: repository has no runtime source tree, package manifest, or test suite
```

## Workspace Baseline

- Base Commit: `77055555fe9ea8a7c4b106e23126368f916cb850`
- Dirty Workspace: Yes. Before this v2 writeback, this case's untracked draft
  files existed and two target files already had a narrow `Required human
  decision` -> `Decision` wording change.
- Existing Changed Files:
  - `skills/_shared/references/loop-protocol.md`
  - `skills/development/docloom-workflow/SKILL.md`
  - `docs/cases/20260703-next-slice-rubric-dogfood/**`
- Run Mode: `inline`.

## Risk Level

`high`: this changes workflow output contract text in Agent Skills. Scope is
bounded to formatting and wording only; authority docs and execution gates stay
unchanged.

## TDD Applicability

- TDD Required: No
- If No, Reason: Pure documentation change.
- Alternative Verification:
  - `git diff --check`
  - Inspect changed-file list.
  - Inspect next-slice output contract text for compact columns and unchanged
    safety gates.
  - Grep active skill docs for stale `Required human decision` next-slice output
    wording.

## Files to Change

- Modify: `skills/_shared/references/loop-protocol.md`
- Modify: `skills/development/docloom-workflow/SKILL.md`
- Create: `docs/cases/20260703-next-slice-rubric-dogfood/execution.md`
- Create: `docs/cases/20260703-next-slice-rubric-dogfood/closure.md`
- Modify: `docs/cases/20260703-next-slice-rubric-dogfood/case_state.yaml`
- Modify: `docs/cases/20260703-next-slice-rubric-dogfood/plan.md`
- Modify: `docs/product/current-state.md`
- Modify: `docs/cases/README.md`

## Acceptance Criteria

| Criteria | Verification |
|---|---|
| Default next-slice candidate table is compact and no longer exposes all five score factors. | Inspect `skills/_shared/references/loop-protocol.md`. |
| Full score factors remain defined for ranking and can be expanded on request or in case evidence. | Inspect `skills/_shared/references/loop-protocol.md`. |
| `docloom-workflow` points next-slice output to the compact contract and uses `Decision:` wording. | Inspect `skills/development/docloom-workflow/SKILL.md`. |
| Safety gates still say recommendations are not execution authorization. | Inspect `skills/_shared/references/loop-protocol.md` and `docloom-workflow/SKILL.md`. |
| Operational docs record the dogfood result and next bottleneck. | Inspect `docs/product/current-state.md`, `docs/cases/README.md`, and closure. |
| Markdown whitespace is valid. | Run `git diff --check`. |

## Tasks

### Task 1: Update compact output contract

**Files:**
- Modify: `skills/_shared/references/loop-protocol.md`
- Modify: `skills/development/docloom-workflow/SKILL.md`

- [x] **Step 1: Replace wide default table**
  - Keep `Score` but remove default score-factor columns from the output table.

- [x] **Step 2: Preserve scoring details as expandable evidence**
  - State that score factors are used for sorting and only expanded on request
    or in case evidence.

- [x] **Step 3: Keep safety gates unchanged**
  - Preserve no-auto-execution and normal `plan-confirm` routing.

### Task 2: Record execution and close the case

**Files:**
- Create: `docs/cases/20260703-next-slice-rubric-dogfood/execution.md`
- Create: `docs/cases/20260703-next-slice-rubric-dogfood/closure.md`
- Modify: `docs/cases/20260703-next-slice-rubric-dogfood/case_state.yaml`
- Modify: `docs/product/current-state.md`
- Modify: `docs/cases/README.md`

- [x] **Step 1: Record execution evidence**
  - Capture changed files, scope boundaries, and verification commands.

- [x] **Step 2: Sync operational docs**
  - Mark the rubric dogfood complete and identify the next bottleneck as a
    follow-up, not authority.

- [x] **Step 3: Close case state**
  - Set final closure fields and `review_risk`.

### Task 3: Verify

**Files:**
- Test: read-only command checks

- [x] **Step 1: Check changed files**
  - Command: `git diff --name-only`
  - Expected result: only planned files changed.

- [x] **Step 2: Check stale wording**
  - Command: `rg "Required human decision" skills`
  - Expected result: no active next-slice output contract uses stale wording.

- [x] **Step 3: Check whitespace**
  - Command: `git diff --check`
  - Expected result: exit 0.

## Risks

- This is still a workflow-output contract change; an independent review could
  catch wording ambiguity, but the scope is intentionally small.

## Documentation Impact

- Skill guidance changes: `loop-protocol.md`, `docloom-workflow/SKILL.md`.
- L2 case evidence: execution and closure under this case.
- Operational discovery inputs: `docs/product/current-state.md` and
  `docs/cases/README.md`.
- No authority docs change.

## Confirmation Log

| When | Confirmed By | Plan Version | Confirmation |
|---|---|---|---|
| 2026-07-03T13:48:54+08:00 | user | 2 | User replied `改` after the concrete compact-output modification proposal. |
