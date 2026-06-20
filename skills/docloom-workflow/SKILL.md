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

- Routing into `plan-confirm`.
- The user explicitly asks to start a persistent case.
- A `context-authority-brief.md` must be persisted for high risk, conflict,
  resume, multi-agent, or cross-session continuity.
- Execution, review input handling, or closure already needs to bind to a case.

Minimal creation:

```text
docs/cases/<case-id>/
docs/cases/<case-id>/case_state.yaml
```

Do not create case docs for status queries, explanation-only requests, one-shot
context checks, explicit skills that do not need persistence, or global
governance setup that is not tied to a case.

## Route Table

| Condition | Route |
|---|---|
| User asks to initialize, rebuild, organize, bridge, archive, or repair docs governance | `setup-doc-governance` |
| `context-authority` verdict is `run_setup_doc_governance` | `setup-doc-governance` |
| User explicitly asks for review or code/docs/test/design review | `review` |
| User asks for status or intent is ambiguous | `status-only` |
| Need resume, authority decision, conflict handling, or pre-plan context gate | `context-authority` |
| Persistent development plan requested and context is enough | `plan-confirm` |
| Approved plan exists and user explicitly asks to execute or continue | `tdd-execute` |
| User asks to sync docs, close, write closure, or execution is complete | `doc-sync-close` |
| Multiple case candidates and no safe assumption | `status-only` with `needs_case_selection` |

Explicit skill invocation wins. If a user names a Doc Loom skill, honor that
skill unless its gate routes back to `docloom-workflow` or another required
owner.

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

- Do not create a case just because the entry skill ran.
- Do not execute just because an approved plan exists.
- Do not run `context-authority` by default.
- Do not auto-trigger `review`, even when `review_risk` is high.
- Do not route or own `grill`.
- Do not write files in status-only mode.
- Do not silently repair state cache conflicts.
- Do not write `base_commit`.
- Do not call a CLI backend.
