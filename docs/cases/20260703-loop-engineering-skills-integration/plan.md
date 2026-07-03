---
case_id: 20260703-loop-engineering-skills-integration
plan_version: 2
status: approved
risk_level: high
approved_by: user
approved_at: 2026-07-03T10:38:44+08:00
base_commit: b5bb4a3093cda0ccf7f7d54eb42680faeb591e75
---

# Implementation Plan

## Goal

Add `docs/cases/README.md` as a derived case dashboard that gives Doc Loom a
human-readable loop discovery entry point without replacing existing case
state, execution evidence, or closure artifacts.

## Non-goals

- Do not add root-level `STATE.md`, `LOOP.md`, `loop-budget.md`, or
  `loop-run-log.md`.
- Do not add a new loop skill, CLI backend, daemon, scheduler, GitHub Action,
  MCP server, or external orchestrator.
- Do not make `docs/cases/README.md` routing truth, execution evidence, or an
  append-only run log.
- Do not auto-trigger `review` or `grill`.
- Do not change Fast-Path, plan approval, or high-risk confirmation semantics.
- Do not stage, commit, push, or publish.

## Context

- Summary / Brief: `context-authority-brief.md`
- Route Verdict: `proceed_to_plan_with_risk`
- Skipped Context Reason: none

## Decisions

- Treat `docs/cases/README.md` as a derived dashboard and discovery index.
- Keep routing truth in each case's `case_state.yaml`.
- Keep execution evidence in `execution.md` and final acceptance in
  `closure.md`.
- Let status-only reads use the dashboard as an entry point, but require case
  artifacts to resolve conflicts or route execution.

## Workspace Baseline

- Base Commit: `b5bb4a3093cda0ccf7f7d54eb42680faeb591e75`
- Dirty Workspace Before Case Creation: no
- Existing Changed Files Before Case Creation: none
- Branch: `main`
- Run Mode: inline

## Risk Level

High.

Reason: this changes workflow and agent policy semantics for case discovery
and shared protocol behavior.

## TDD Applicability

- TDD Required: No
- If No, Reason: Agent Skill and Markdown protocol update with no runtime source
  tree or automated behavior test suite.
- Alternative Verification:
  - `git diff --check`
  - grep updated docs for forbidden backend/scheduler/orchestrator terms
  - inspect skill changes for routing ownership and manual-only review/grill
  - advisory loop readiness audit from `loop-engineering`

## Files to Change

Create:

- `docs/cases/README.md`
- `docs/cases/20260703-loop-engineering-skills-integration/execution.md`
- `docs/cases/20260703-loop-engineering-skills-integration/closure.md`

Modify:

- `skills/_shared/references/shared-protocol.md`
- `skills/development/docloom-workflow/SKILL.md`
- `skills/development/doc-sync-close/SKILL.md`
- `docs/authority/workflow/development-flow.md`
- `docs/ssot-map.md`
- `docs/cases/20260703-loop-engineering-skills-integration/case_state.yaml`

## Acceptance Criteria

| Criteria | Planned Verification |
|---|---|
| `docs/cases/README.md` exists and clearly says it is a derived dashboard, not source of truth. | Inspect the file. |
| The dashboard can replace root-level loop `STATE.md` as the human case overview without replacing `case_state.yaml`. | Inspect dashboard and shared protocol text. |
| The dashboard can replace root-level loop run-record summaries without becoming execution evidence or an append-only log. | Inspect dashboard and shared protocol text. |
| `docloom-workflow` may use the dashboard in status-only mode but must route from case artifacts. | Inspect `docloom-workflow/SKILL.md`. |
| `doc-sync-close` knows to update the dashboard as derived output after closure. | Inspect `doc-sync-close/SKILL.md`. |
| Authority and SSOT docs identify the dashboard at the correct derived/index layer. | Inspect `development-flow.md` and `docs/ssot-map.md`. |
| No backend, daemon, scheduler, new loop skill, or root loop scaffold is introduced. | Inspect changed files and grep. |
| Markdown whitespace is valid. | Run `git diff --check`. |

## Tasks

### Task 1: Add cases dashboard

**Files:**
- Create: `docs/cases/README.md`

- [x] Add active cases, waiting-on-human, recently closed, and follow-up
  candidate sections.
- [x] State that source of truth remains per-case artifacts.

### Task 2: Wire dashboard into protocol and skills

**Files:**
- Modify: `skills/_shared/references/shared-protocol.md`
- Modify: `skills/development/docloom-workflow/SKILL.md`
- Modify: `skills/development/doc-sync-close/SKILL.md`

- [x] Define `docs/cases/README.md` as an optional derived index.
- [x] Allow status-only reads to use it as a dashboard, not routing truth.
- [x] Require closure to refresh it when case closure changes the dashboard.

### Task 3: Sync authority references

**Files:**
- Modify: `docs/authority/workflow/development-flow.md`
- Modify: `docs/ssot-map.md`

- [x] Add minimal authority references without duplicating protocol detail.

### Task 4: Verify and close

**Files:**
- Create: `execution.md`
- Create: `closure.md`
- Modify: `case_state.yaml`

- [x] Run planned verification.
- [x] Record final evidence and closure.

## Confirmation Log

| When | Confirmed By | Plan Version | Confirmation |
|---|---|---|---|
| 2026-07-03T10:38:44+08:00 | user | 2 | "可以，执行" after approving the `docs/cases/README.md` derived dashboard approach. |
