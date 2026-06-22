---
name: doc-sync-close
description: Close an existing document-driven workflow case by syncing docs and writing closure. Use after execution, or when a case is blocked, cancelled, paused, superseded, abandoned, or otherwise ending. Updates L2 case docs, safe mechanical L3 derived docs, proposes authority changes or applies explicitly confirmed narrow authority patches, records tests, acceptance criteria, findings, review_risk, evidence gaps, confirmations, follow-ups, writes closure.md, and sets closure_status.
---

# doc-sync-close

Synchronize task documents and close, pause, block, cancel, supersede, or
abandon an existing Doc Loom case.

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
- `GOVERNANCE_PLAN.md` only when checking existing blocked decisions or
  recording governance follow-up.

Do not create case id or case docs. If the user asks for documented closure
without case docs, route back to `docloom-workflow` for minimal case identity.

## Closure Status

Use the closure status set from `references/shared-protocol.md`.

Additional guidance:
- `Done`: all acceptance criteria verified as met.
- `Done with Caveats`: main goal complete, residual risk or follow-up remains.
- `Blocked`/`Cancelled`/`Superseded`/`Paused`/`Abandoned`: see shared-protocol
  meanings. Record the specific reason in `closure.md`.

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
    point.

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
- If Artifact Policy required `execution.md` but it is absent, record an
  evidence gap and do not use unqualified `Done`.
- If Artifact Policy did not require `execution.md`, closure may carry the full
  verification evidence itself.
- Missing non-required `execution.md` does not automatically downgrade closure.

## Knowledge Changes

Classify possible knowledge changes as:

- `authority_candidate`
- `regression_candidate`
- `runbook_candidate`
- `derived_sync`
- `case_local`
- `none`

Do not silently promote knowledge into authority. Reproducible bugs usually
become test follow-ups. Repeated operations usually become runbook or automation
proposals. One-off case learning stays in closure.

## Authority Handling

Default: write a proposed change in closure.

Apply an authority patch only when the user explicitly confirms a narrow patch
and all conditions in `references/doc-update-rules.md` are satisfied.

Structural, high-risk, conflict-heavy, new authority-area, bridge/archive, or
lifecycle changes go to `GOVERNANCE_PLAN.md` or a new case.

## State Update

Write `closure.md` before updating `case_state.yaml`.

State shape:

```yaml
phase: closed
closure_status: Done with Caveats
closure_path: docs/cases/<case-id>/closure.md
last_updated: 2026-06-17T00:00:00+08:00
```

If state update fails, `closure.md` remains the truth record, but do not claim
workflow fully synced.

review_risk is consumed at closure. Do not change it during closure unless new
closure-process evidence changes the assessment. Record the consumed value and
its resolution in `closure.md`.

## Gates

- Authority update requires user confirmation. → Route: doc-sync-close. Reason: authority change requires explicit user confirmation. Required input: user confirmation of concrete document and change.
- Unmet acceptance criteria cannot be `Done`. → Route: doc-sync-close. Reason: acceptance criteria not met. Required input: user decision — adjust criteria, accept caveats, or block.
- User-provided Critical or Important findings that affect acceptance cannot be
  ignored.
- High `review_risk` with insufficient evidence cannot be unqualified `Done`.
- Missing `execution.md` required by Artifact Policy cannot be unqualified
  `Done`. → Route: doc-sync-close. Reason: execution evidence missing. Required input: evidence gap record, user decision on Done with Caveats or Blocked.
- Case docs that end, pause, block, cancel, or supersede must get `closure.md`.
- Do not update `case_state.yaml` to closed before closure is written.
- Do not modify plan semantics; at most append a closure link or short note.
- Do not modify code, tests, dependencies, scripts, lockfiles, CI, or acceptance
  criteria to make closure pass.
