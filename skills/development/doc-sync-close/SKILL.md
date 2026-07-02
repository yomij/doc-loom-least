---
name: doc-sync-close
description: Sync docs and close a Doc Loom case. Use after execution, or when a case is blocked, cancelled, paused, superseded, or abandoned.
---

# doc-sync-close

Read when trigger condition is met:

- `references/shared-protocol.md` when: writing closure_status, checking
  artifact policy, or resolving authority order.
- `references/doc-update-rules.md` when: any L1 authority doc may change, L3
  derived docs need sync, or writing the authority change table.

## Inputs

- `plan.md`, especially Goal, Non-goals, Decisions, Acceptance Criteria,
  Documentation Impact, and Plan Amendments.
- `execution.md` when it exists or Artifact Policy requires it.
- Test and verification results.
- Current diff.
- `review_risk` from execution or user context.
- User-provided findings, including pasted `review` output, if any.
- Related authority and L3 docs.
- `case_state.yaml`.
- Governance plans only when checking existing blocked decisions or recording
  governance follow-up. See `references/doc-update-rules.md` for path conventions.

Per shared protocol Case Identity, this skill consumes existing case identity.
If the user asks for documented closure without case docs, route back to
`docloom-workflow` for minimal case identity.

## Closure Status

Use the closure status set from `references/shared-protocol.md`.

Key distinctions:
- `Done`: all acceptance criteria verified as met.
- `Done with Caveats`: main goal complete, residual risk or follow-up remains.
- For other statuses, record the specific reason in `closure.md`.

## Workflow

1. Determine closure status from acceptance evidence, user instruction, and
   execution state.
2. Verify each acceptance criterion as `met`, `partially_met`, `not_met`,
   `not_verified`, or `out_of_scope`.
3. Summarize code changes, tests, unrun checks, and risks.
4. Identify knowledge changes without turning closure into a governance
   interview.
5. Sync L2 operational docs.
6. Mechanically sync safe L3 derived docs when allowed.
7. Handle authority changes as proposals or confirmed narrow patches.
8. Write `closure.md`.
9. After `closure.md` is written and checked, update `case_state.yaml`.
10. Write `handoff.md` only for `Paused`, `Blocked`, or another future resume
    point. Use `templates/handoff.md`.

## Acceptance Mapping

| Acceptance result | Closure handling |
|---|---|
| All `met` | May be `Done`; use `Done with Caveats` if residual risk or unconfirmed authority proposal remains. |
| Any `not_verified` | Verify, get user confirmation, record caveat, or choose `Blocked` / `Done with Caveats`. |
| Any `partially_met` | Not `Done` unless user accepts the remainder as follow-up; then use `Done with Caveats`. |
| Any `not_met` | Usually `Blocked`, `Paused`, `Cancelled`, or `Superseded`; only user goal change can allow caveated closure. |
| `out_of_scope` | Does not block closure, but explain why it is out of scope. |

Required execution evidence:

- If `execution.md` exists, use it as execution evidence.
- If `execution.md` records a material deviation, verify it returned to
  `plan-confirm` or has explicit user confirmation before unqualified `Done`.
- If Artifact Policy required `execution.md` but it is absent, record an
  evidence gap and do not use unqualified `Done`.
- If Artifact Policy did not require `execution.md`, closure may carry the full
  verification evidence itself.
- Missing non-required `execution.md` does not automatically downgrade closure.

Closure owns final acceptance status and final verification summary. Do not
repeat the full planned task list from `plan.md` or interim process narrative
from `execution.md` unless that evidence is needed to justify the final status.

## Knowledge Changes

Classify possible knowledge changes as:

- `authority_candidate`
- `ADR_candidate`
- `regression_candidate`
- `runbook_candidate`
- `derived_sync`
- `case_local`
- `none`

Do not silently promote knowledge into authority. Classify the knowledge change
and record the authority proposal, follow-up, derived sync, or case-local note.

Before closing, check whether the final diff or confirmed decisions create a
reusable default that may constrain future work. If so, record it as
`authority_candidate`, `ADR_candidate`, or `runbook_candidate`. If it is
classified as `case_local`, explain why it does not constrain future work. An
unresolved `authority_candidate` is residual risk; use `Done with Caveats`
instead of unqualified `Done`.

## Authority Handling

Default: write a proposed change in closure.

Apply an authority patch only when the user explicitly confirms a narrow patch
and all conditions in `references/doc-update-rules.md` are satisfied.

Structural, high-risk, conflict-heavy, new authority-area, bridge/archive, or
lifecycle changes go to a new governance batch or a new case.

## State Update

Write `closure.md` before updating `case_state.yaml`.

State shape:

```yaml
phase: closed
closure_status: Done with Caveats
closure_path: docs/cases/<case-id>/closure.md
last_updated: <timestamp>
```

If state update fails, `closure.md` remains the truth record, but do not claim
workflow fully synced.

review_risk is consumed at closure. Do not change it during closure unless new
closure-process evidence changes the assessment. Record the consumed value and
its resolution in `closure.md`.

## Gates

- Authority update requires concrete user confirmation -> wait for user input.
- Unmet acceptance criteria cannot be `Done` -> wait for user input.
- User-provided Critical or Important findings that affect acceptance cannot be
  ignored.
- High `review_risk` with insufficient evidence cannot be unqualified `Done`.
- Unresolved material deviation cannot be unqualified `Done`.
- Missing `execution.md` required by Artifact Policy cannot be unqualified
  `Done` -> wait for user input.
- Case docs that end, pause, block, cancel, or supersede must get `closure.md`.
- Do not update `case_state.yaml` to closed before closure is written.
- Do not modify plan semantics; at most append a closure link or short note.
- Do not modify code, tests, dependencies, scripts, lockfiles, CI, acceptance
  criteria, or plan semantics to make closure pass.
