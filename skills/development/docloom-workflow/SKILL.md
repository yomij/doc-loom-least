---
name: docloom-workflow
description: Default entry for Doc Loom Least. Use when a user asks for a docs-first workflow but does not name a specific skill, asks for status/continue/where-we-are routing, or asks what next product or development slice should be discovered.
---

# docloom-workflow

Route to the owning skill; do not replace stage skills.

Read shared rules first when routing or creating case state:
`references/shared-protocol.md`.

Read `references/loop-protocol.md` when the user asks for case candidates,
loop-style discovery, next work, next feature, or next-slice discovery.

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
- Use `docs/cases/README.md`, when present, only to narrow case artifact reads;
  verify phase, next skill, and required input from case artifacts.
- Output current phase, evidence, route decision, next skill, blocking issue,
  and one clarifying question if needed.

For multiple case candidates or follow-ups, use the Case Candidates table from
`references/loop-protocol.md`; this is output shape only, not a new stage.

## Next-Slice Discovery

Use this mode when the user asks what to build next, what the next feature slice
should be, or asks the agent to discover the next iteration target.

Inputs are `docs/product/current-state.md` or equivalent current-turn facts,
case dashboard and closure follow-ups when present, minimal git state, and
targeted repo evidence only when needed to support candidate claims.

If required product-state fields are missing, do not recommend a new task. Use
the blocking output from `references/loop-protocol.md`; if creating the file is
the next user-approved action, use `templates/product-current-state.md`.

Output Next Slice Candidates using the table in `references/loop-protocol.md`.
Recommend exactly one candidate with one-sentence reasoning, or recommend no
candidate when evidence is insufficient.

Next-slice discovery is read-only. Create no case or plan until the user selects
a candidate; then route through normal case identity and `plan-confirm`.

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

Do not create case docs under the conditions listed in
`references/shared-protocol.md` -> Case Creation Boundary.

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
| 6 | User asks for next-slice discovery without selecting a candidate | `status-only` with Next Slice Candidates |
| 7 | User selects a next-slice candidate and persistent work is needed | `plan-confirm` after case identity and context/skip reason exist |
| 8 | Persistent work needing a context gate: resume, authority, conflict, or High-Risk Topic | `context-authority` |
| 9 | Low-risk, ≤ 3 files, ≤ 20 lines diff, no conflict, no cross-session need — fast-path conditions all hold | `plan-confirm` (fast-path) |
| 10 | Persistent work with inline context or skipped-context reason | `plan-confirm` |
| 11 | Approved plan exists and user explicitly asks to execute or continue | `tdd-execute` |
| 12 | User asks to sync docs, close, write closure, or execution is complete | `doc-sync-close` |

Explicit skill invocation wins. If a user names a Doc Loom skill, honor that
skill unless its gate routes back to `docloom-workflow` or another required
owner.

For fast-path (row 9): `plan-confirm` writes a minimal approved plan with
`approved_by: fast-path`, a Confirmation Log row, fast-path eligibility, and no
task-level TDD breakdown. See `references/shared-protocol.md` → Fast-Path.

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

For next-slice discovery, append:

```text
Next Slice Candidates:
Recommended:
Required human decision:
```

Do not create route artifacts. If a route decision affects a plan, it belongs
in `plan.md` context, assumptions, or risks. If it affects closure, it belongs
in `closure.md`.

## Gates

- Do not create a case just because the entry skill ran; status-only remains
  read-only.
- Approved plan without an execution request -> wait for user input, unless it
  is a Fast-Path plan approved under the current user request.
- Do not run `context-authority` for status, explanation-only, or clearly
  low-risk one-step work.
- Do not enter `plan-confirm` for persistent work without a context summary,
  context brief, or explicit low-risk skipped-context reason.
- Do not auto-trigger `review`, even when `review_risk` is high.
- Do not route or own `grill`; it requires explicit invocation.
- Do not write files in status-only mode.
- Do not execute or plan a next-slice candidate until the user selects it.
- Do not silently repair state cache conflicts.
- Do not write `base_commit` or `risk_level`; these belong to `plan.md` frontmatter.
- Do not call a CLI backend.
