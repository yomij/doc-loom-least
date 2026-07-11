---
name: docloom-workflow
description: Human-facing default entry for Doc Loom Least. Use for persistent feature, bug, refactor, workflow, or agent-policy work; docs-first development; case status or continuation; and next-slice discovery when the user does not explicitly name another owning skill. Skip explanation-only, standalone review/grill, and clearly one-shot low-risk local edits.
---

# docloom-workflow

Act as the one public doorway and route internally to the owning skill; do not
replace stage ownership or require the user to know stage skill names.

Read shared rules first when routing or creating case identity:
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
- Read only the git state and necessary case docs.
- Use `docs/cases/README.md`, when present, only to narrow case artifact reads;
  derive current status and next action from case artifacts.
- Output human status, evidence, what can happen next, the decision needed, and
  one clarifying question if needed. Include internal phase/skill names only for
  diagnosis or when the user asks for workflow detail.

For multiple case candidates or follow-ups, use the Case Candidates table from
`references/loop-protocol.md`; this is output shape only, not a new stage.

## Next-Slice Discovery

Use this mode when the user asks what to build next, what the next feature slice
should be, or asks the agent to discover the next iteration target.

Inputs are `docs/product/current-state.md` or equivalent current-turn facts,
case dashboard and closure follow-ups when present, minimal git state, and
targeted repo evidence only when needed to support candidate claims.

When product-state fields are missing, first infer discoverable facts from
active authority, current implementation, README, case follow-ups, and current
turn facts. Label inferred facts and confidence. Block only when a missing human
fact would materially change candidate ranking; see `references/loop-protocol.md`.

Output Next Slice Candidates using the compact table in
`references/loop-protocol.md`. Recommend exactly one candidate with
one-sentence reasoning, or recommend no candidate when evidence is insufficient.

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

Minimal creation is the directory `docs/cases/<case-id>/`. The routed owner
writes the first authoritative artifact in the same flow; do not leave an empty
case directory as durable state.

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
| 11 | Current-plan confirmation without hold, or approved plan with current execute/continue intent | `tdd-execute` |
| 12 | User asks to sync docs, close, write closure, or execution is complete | `doc-sync-close` |

Explicit skill invocation wins. If a user names a Doc Loom skill, honor that
skill unless its gate routes back to `docloom-workflow` or another required
owner.

For fast-path (row 9): `plan-confirm` writes a minimal approved plan with
`approved_by: fast-path`, a Confirmation Log row, fast-path eligibility, and no
task-level TDD breakdown or separate plan commit. The approved green change,
compact review, closure, and final status form one local completion commit. See
`references/shared-protocol.md` → Fast-Path.

## Artifact Policy

Apply the shared Artifact Policy. Stage skills write their own artifacts.
`docloom-workflow` only owns case identity; current status is carried by the
stage artifacts themselves.

## Output Contract

Default human response:

```text
Status:
What I found or completed:
What happens next:
Decision needed:
Local Git effect: <only when relevant>
```

For next-slice discovery, use compact output instead of repeating the full
default response unless blocked:

```text
Status:
Next Slice Candidates:
Recommended:
Decision:
```

Do not expose internal routing by default or create route artifacts. If a route
decision affects a plan, it belongs in `plan.md` context, assumptions, or risks.
If it affects closure, it belongs in `closure.md`.

## Gates

- Do not create a case just because the entry skill ran; status-only remains
  read-only.
- Current-plan approval -> same-turn execution unless plan-only, hold, revise,
  or review first.
- Later-discovered approved plan -> wait unless current execute, continue, or
  reconfirm intent exists.
- Fast-Path remains limited to verified low-risk current requests.
- Do not run `context-authority` for status, explanation-only, or clearly
  low-risk one-step work.
- Do not enter `plan-confirm` for persistent work without a context summary,
  context brief, or explicit low-risk skipped-context reason.
- Do not auto-trigger ad-hoc `review`, even when `review_risk` is high.
  `tdd-execute` owns the approved Post-execution review gate for eligible cases;
  it is not a router stage.
- Do not route or own `grill`; it requires explicit invocation.
- Do not write files in status-only mode.
- Do not execute or plan a next-slice candidate until the user selects it.
- Do not write `base_commit` or `risk_level`; these belong to `plan.md` frontmatter.
- Do not call a CLI backend.
