---
case_id: 20260703-next-slice-discovery-integration
plan_version: 1
status: approved
risk_level: high
approved_by: user
approved_at: 2026-07-03T11:10:00+08:00
base_commit: b5bb4a3093cda0ccf7f7d54eb42680faeb591e75
---

# Implementation Plan

## Goal

Complete the loop-engineering integration by adding bounded next-slice
discovery to Doc Loom Least while preserving existing case discovery, plan
confirmation, execution, closure, and manual review/challenge gates.

## Non-goals

- Do not add root `STATE.md`, `LOOP.md`, `loop-budget.md`, or
  `loop-run-log.md`.
- Do not add a loop skill group, CLI backend, daemon, scheduler, MCP server,
  GitHub Actions loop runner, or orchestrator.
- Do not auto-execute discovered candidates.
- Do not auto-trigger `review` or `grill`.
- Do not make product-state notes override authority docs.

## Context

- Prior case: `20260703-loop-engineering-skills-integration`.
- Follow-up: `docs/cases/README.md` F1 requested next-slice discovery only
  after a concrete product-state contract exists.
- Current user request: complete the full plan.

## Decisions

- Add `skills/_shared/references/loop-protocol.md` as implementation guidance.
- Keep case discovery owned by existing status-only routing and case artifacts.
- Add `docs/product/current-state.md` as the operational input for next-slice
  discovery.
- Let `docloom-workflow` recommend candidates but not execute them.
- Let `plan-confirm` record the selected discovery candidate when the user
  chooses one.

## Workspace Baseline

- Base Commit: `b5bb4a3093cda0ccf7f7d54eb42680faeb591e75`
- Branch: `main`
- Run Mode: inline

## Risk Level

High.

Reason: workflow and agent policy semantics are changing.

## TDD Applicability

- TDD Required: No
- Reason: Agent Skill and Markdown protocol update with no runtime source tree
  or automated behavior suite.
- Alternative Verification: text inspection, `git diff --check`, grep for
  forbidden automation, and advisory loop audit.

## Files to Change

- `skills/_shared/references/loop-protocol.md`
- `docs/product/current-state.md`
- `skills/development/docloom-workflow/templates/product-current-state.md`
- `skills/_shared/references/shared-protocol.md`
- `skills/development/docloom-workflow/SKILL.md`
- `skills/development/context-authority/SKILL.md`
- `skills/development/plan-confirm/SKILL.md`
- `skills/development/tdd-execute/SKILL.md`
- `skills/development/doc-sync-close/SKILL.md`
- `docs/authority/workflow/development-flow.md`
- `docs/ssot-map.md`
- `docs/cases/README.md`

## Acceptance Criteria

| Criteria | Planned Verification |
|---|---|
| Loop protocol exists and keeps case discovery under existing Doc Loom routing. | Inspect `loop-protocol.md`. |
| Product current state input exists and is explicitly non-authoritative. | Inspect `docs/product/current-state.md`. |
| `docloom-workflow` supports next-slice discovery and candidate output without execution. | Inspect `docloom-workflow/SKILL.md`. |
| `plan-confirm` records selected candidates in plans. | Inspect `plan-confirm/SKILL.md`. |
| `tdd-execute` cannot treat a recommendation as execution authorization. | Inspect `tdd-execute/SKILL.md`. |
| `doc-sync-close` can update product state and dashboard follow-ups after closure. | Inspect `doc-sync-close/SKILL.md`. |
| No forbidden backend/scheduler/orchestrator or auto-review behavior is introduced. | Grep changed docs and skills. |
| Markdown whitespace is valid. | Run `git diff --check`. |

## Confirmation Log

| When | Confirmed By | Plan Version | Confirmation |
|---|---|---|---|
| 2026-07-03T11:10:00+08:00 | user | 1 | "完成完整方案" confirming the previously discussed full integration scope. |
