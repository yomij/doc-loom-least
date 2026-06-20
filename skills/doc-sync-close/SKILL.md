---
name: doc-sync-close
description: Close an existing document-driven workflow case by syncing docs and writing closure. Use after execution, or when a case is blocked, cancelled, paused, superseded, abandoned, or otherwise ending. Updates L2 case docs, safe mechanical L3 derived docs, proposes authority changes or applies explicitly confirmed narrow authority patches, records tests, acceptance criteria, findings, review_risk, evidence gaps, confirmations, follow-ups, writes closure.md, and sets closure_status.
---

# doc-sync-close

Synchronize task documents and close, pause, block, cancel, supersede, or
abandon an existing Doc Loom case.

Read shared rules when needed: `../_shared/references/shared-protocol.md`.
Read document update rules when authority or L3 docs may change:
`references/doc-update-rules.md`.

## Inputs

- `plan.md`, especially Goal, Non-goals, Decisions, Acceptance Criteria,
  Documentation Impact, and Plan Amendments.
- `execution.md` when it exists or Artifact Policy requires it.
- Test and verification results.
- Current diff.
- `review_risk` from execution or user context.
- User-provided findings, if any.
- Related authority and L3 docs.
- `case_state.yaml`.
- `GOVERNANCE_PLAN.md` only when checking existing blocked decisions or
  recording governance follow-up.

Do not create case id or case docs. If the user asks for documented closure
without case docs, route back to `docloom-workflow` for minimal case identity.

## Closure Status

Use exactly one:

| Status | Meaning |
|---|---|
| `Done` | All acceptance criteria are met. |
| `Done with Caveats` | Main goal is complete with residual risk or follow-up. |
| `Blocked` | Dependency, permission, information, or verification prevents progress. |
| `Cancelled` | User or project decided to cancel. |
| `Superseded` | Another task or approach replaces this one. |
| `Paused` | Temporarily stopped and may resume later. |
| `Abandoned` | Not expected to resume directly. |

Do not mark `Done` when acceptance criteria are unmet or unverified. High
`review_risk` does not automatically block closure if evidence is sufficient;
record residual risk instead.

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

## Gates

- Authority update requires user confirmation.
- Unmet acceptance criteria cannot be `Done`.
- User-provided Critical or Important findings that affect acceptance cannot be
  ignored.
- High `review_risk` with insufficient evidence cannot be unqualified `Done`.
- Case docs that end, pause, block, cancel, or supersede must get `closure.md`.
- Do not update `case_state.yaml` to closed before closure is written.
- Do not modify plan semantics; at most append a closure link or short note.
- Do not modify code, tests, dependencies, scripts, lockfiles, CI, or acceptance
  criteria to make closure pass.
