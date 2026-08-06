<!--
Required: Goal, Guardrails/Non-goals, Acceptance, Escalation Triggers, Context,
Workspace Baseline, Risk/assurance, TDD, likely Files, Tasks, Confirmation Log.
Guarded plans also require Human Approval Summary and Post-Execution Review.
Add Decisions, Assumptions, Amendments, Tests, Commits, Risks, or Docs Impact
only when material. Record planned, not final, evidence. Remove unused headings.

Compact terminal close (no closure.md): doc-sync-close sets final_status and
closed_at on this frontmatter and may add a short ## Final status body.
Guarded terminal close uses thin closure.md instead.
-->
---
case_id:
plan_version: 1
status: draft
risk_level:
approved_by:
approved_at:
base_commit:
final_status:
closed_at:
---

# Implementation Plan

## Human Approval Summary (guarded only)

- Outcome:
- Material scope:
- Decision needed:
- Expected local Git actions / commit count:
- Likely interruptions:
- Excluded actions:

## Goal

## Guardrails / Non-goals

## Context

- Summary / brief / valid skip:
- Route verdict:

## Workspace Baseline

- Base commit:
- Branch / run mode:
- Dirty or existing changes:

## Risk Level

- Guarded assurance required:
- Reason:

## TDD Applicability

- TDD Required:
- If No, reason and alternative verification:

## Files to Change

## Acceptance Criteria

| Criterion | Planned verification |
|---|---|

## Escalation Triggers

- Changed outcome, non-goal, or acceptance:
- Risk / authority / public contract:
- Dependency, lockfile, CI, schema, or config contract:
- External resource or irreversible action:
- Other protected boundary:

## Tasks

### Task 1: Name

**Files:**

- [ ] Characterize/fail as required.
- [ ] Implement the smallest change.
- [ ] Verify and record checkpoint.

## Confirmation Log

| When | Confirmed By | Plan Version | Confirmation |
|---|---|---:|---|

## Final status

<!-- Compact close only. Guarded cases use thin closure.md. Keep short:
outcome, acceptance summary, residuals/follow-ups, evidence pointers. -->
