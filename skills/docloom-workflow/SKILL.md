---
name: docloom-workflow
description: Public automatic entry for Doc Loom Least. Use as the default entry when a user asks for a docs-first change workflow but does not explicitly invoke a specific Doc Loom skill. Resolves current case status, routes to setup-doc-governance, context-authority, plan-confirm, tdd-execute, doc-sync-close, or explicitly requested conversation-only review, owns delayed case creation and case_id generation, consumes review_risk signals, and applies the global Artifact Policy without becoming a heavy orchestrator.
---

# docloom-workflow

Use the minimum path that safely resolves the current request. Route; do not
replace stage skills.

Read shared rules first when routing or creating case state:
`../_shared/references/shared-protocol.md`.

## Start

Run the minimal workspace checks:

```bash
git rev-parse --show-toplevel
git branch --show-current
git status --short
```

Run `git diff --name-only` only when the workspace is dirty, the user asks to
resume, current changes must be matched to a case, or the next owner needs a
baseline decision.

If this is not a git repository, record `git_available: false` and continue
with status-only, document reading, or pure document workflow. Do not write
`base_commit`; `plan-confirm` owns it.

## Status-Only Mode

Use status-only when the user asks for status, "where are we", "can we
continue", or when intent is ambiguous.

Status-only behavior:

- Do not create a case.
- Do not modify files.
- Do not repair `case_state.yaml`.
- Read only the git state and necessary case docs.
- Output current phase, evidence, route decision, next skill, blocking issue,
  and one clarifying question if needed.

## Case Identity

`docloom-workflow` is the case identity owner. Resolve or create case id using
the order in the shared protocol.

Create minimal case docs only when:

- Routing into `plan-confirm` after context is gathered or explicitly skipped.
- The user explicitly asks to start a persistent case.
- A `context-authority-brief.md` must be persisted for high risk, conflict,
  resume, multi-agent, or cross-session continuity.
- Execution, review input handling, or closure already needs to bind to a case.

Minimal creation:

```text
docs/cases/<case-id>/
docs/cases/<case-id>/case_state.yaml
```

Use `templates/case_state.yaml` for the initial case state. It contains the
minimum routing fields: `case_id`, `phase`, `case_docs`, `closure_status`,
`closure_path`, `last_updated`.

Do not create case docs for status queries, explanation-only requests, one-shot
context checks, explicit skills that do not need persistence, or global
governance setup that is not tied to a case.

## Route Table

Evaluate in order. First matching condition wins. Explicit skill invocation
overrides all conditions below.

| # | Condition | Route |
|---:|---|---|
| 1 | User asks to initialize, rebuild, organize, bridge, archive, or repair docs governance | `setup-doc-governance` |
| 2 | `context-authority` verdict is `run_setup_doc_governance` | `setup-doc-governance` |
| 3 | User explicitly asks for review or code/docs/test/design review | `review` |
| 4 | Multiple case candidates and no safe assumption | `status-only` with `needs_case_selection` |
| 5 | User asks for status or intent is ambiguous | `status-only` |
| 6 | Need resume, authority decision, conflict handling, or high-risk work | `context-authority` |
| 7 | Low-risk, ≤ 3 files, ≤ 20 lines diff, no conflict, no cross-session need — fast-path conditions all hold | `plan-confirm` (fast-path) |
| 8 | Persistent work needing a context gate but not yet high-risk enough for full context-authority | `context-authority` |
| 9 | Persistent work with inline context or skipped-context reason | `plan-confirm` |
| 10 | Approved plan exists and user explicitly asks to execute or continue | `tdd-execute` |
| 11 | User asks to sync docs, close, write closure, or execution is complete | `doc-sync-close` |

Explicit skill invocation wins. If a user names a Doc Loom skill, honor that
skill unless its gate routes back to `docloom-workflow` or another required
owner.

For fast-path (row 7): `plan-confirm` writes a minimal plan with
`approved_by: fast-path` and no task-level TDD breakdown. See
`../_shared/references/shared-protocol.md` → Fast-Path.

## Artifact Policy

Apply the shared Artifact Policy. Stage skills write their own artifacts.
`docloom-workflow` only creates the initial case state and records artifact
decisions when a case is created.

Allowed case-state artifact cache:

```yaml
artifacts:
  context_brief: inline
  handoff: not_needed
  execution: conditional
  closure: required
```

## Output Contract

Default response:

```text
Current phase:
Evidence:
Route decision:
Next skill:
Blocking issue:
```

Do not create route artifacts. If a route decision affects a plan, it belongs
in `plan.md` context, assumptions, or risks. If it affects closure, it belongs
in `closure.md`.

## Gates

- Do not create a case just because the entry skill ran. → Route: docloom-workflow. Reason: status-only. Required input: user intent.
- Do not execute just because an approved plan exists. → Route: docloom-workflow. Reason: execution requires explicit user request. Required input: user intent.
- Do not run `context-authority` for status, explanation-only, or clearly
  low-risk one-step work.
- Do not enter `plan-confirm` for persistent work without a context summary,
  context brief, or explicit low-risk skipped-context reason.
- Do not auto-trigger `review`, even when `review_risk` is high. → Route: docloom-workflow. Reason: review requires explicit user request. Required input: user intent.
- Do not route or own `grill`. → Route: docloom-workflow. Reason: grill is conversation-only, requires explicit invocation. Required input: user intent.
- Do not write files in status-only mode.
- Do not silently repair state cache conflicts.
- Do not write `base_commit` or `risk_level`; these belong to `plan.md` frontmatter.
- Do not call a CLI backend.
