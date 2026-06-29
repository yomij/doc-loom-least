---
name: context-authority
description: Gather minimal context and resolve authority before planning. Use before plan-confirm when a case needs a context gate, on resume, or when authority conflicts may affect the plan.
---

# context-authority

Read the smallest relevant context before planning. Do not write an
implementation plan, modify files, create case identity, or resolve authority
conflicts.

Read when trigger condition is met:

- `references/shared-protocol.md` when: resolving case identity order,
  checking fact authority order, checking closure status set, checking artifact
  policy, or checking execution instruction order.
- `references/context-resolution.md` when: classifying case intent, resolving
  case identity, or deciding run mode.
- `references/retrieval-routing.md` when: deciding which governance docs to
  read, filtering authority sources, or writing context source notes.

## Start

If `docloom-workflow` already ran workspace checks in this session, reuse that
output. Otherwise run the minimal workspace checks:

```bash
git rev-parse --show-toplevel
git branch --show-current
git status --short
```

Run `git diff --name-only` only when the workspace is dirty, the user asks to
resume, the current diff matters, or `plan-confirm` will need a baseline
summary.

If git is unavailable, record `git_available: false`, continue reading relevant
docs, and do not record `base_commit`.

## Trigger Boundary

Use this skill when planning needs a context gate, resume needs current case
state, or authority/code/test conflicts may affect the plan.

Skip it for:

- Clearly low-risk one-step changes.
- Mechanical documentation edits.
- Explanation-only requests that will not enter planning.
- Local tasks that do not require authority, code, or test cross-checking.

Use `setup-doc-governance` instead for documentation governance initialization.
Use `review` only when the user explicitly asks for review.

## Workflow

1. Classify case intent.
2. Resolve workspace.
3. Resolve existing case context or proposed slug.
4. Read governance and authority docs by relevance.
5. Read existing case docs only when resuming or when a case is explicit.
6. Find relevant code and tests only when the intent requires them.
7. Record included and excluded sources with reasons.
8. Detect conflicts and classify risk.
9. Output a route verdict.

## Output

Default output is an inline context summary for `plan-confirm`.

Write `docs/cases/<case-id>/context-authority-brief.md` only when:

- Existing case docs need recovery or continuation.
- Task is high risk.
- Authority/code/tests/case docs conflict.
- Multi-agent or cross-session continuity needs stable context.
- User explicitly requests a persisted brief.

If case identity does not exist, do not create it. Output `proposed_case_slug`
or `case_intent`; `docloom-workflow` creates the actual case.

## Route Verdicts

Output exactly one:

| Verdict | Meaning |
|---|---|
| `proceed_to_plan` | Context is sufficient and no blocking conflict was found. |
| `proceed_to_plan_with_risk` | Planning may continue, but weak authority, missing governance, or non-blocking drift must be recorded. |
| `needs_user_decision` | A key fact or decision is missing. |
| `needs_case_selection` | Multiple case candidates exist and no safe assumption is possible. |
| `run_setup_doc_governance` | Missing or conflicting governance facts must be resolved first. |
| `blocked_by_authority_conflict` | Blocking authority/code/test conflict exists. |

## Conflict Handling

Blocking conflicts include:

- Any High-Risk Topic conflict; see `references/shared-protocol.md`.
- Recently owner-confirmed fact conflict.
- Code and tests disagree.
- Test coverage is missing and impact is high.
- Workflow or agent policy change lacks confirmation.

Non-blocking drift may proceed to plan when it is internal, low risk, not
public contract, not active authority, and code/tests agree. Record the
assumption and risk.

## Gates

- No context, no plan; output `needs_user_decision` or `needs_case_selection`.
- Do not enter `plan-confirm` with a blocking conflict.
- Do not create a case id or case docs. → Route: docloom-workflow. Reason: case creation owned by docloom-workflow. Required input: user request for case initialization.
- Read-only; do not modify any file.
- Do not use archived, superseded, needs-refresh, derived, or scratch docs as
  current facts unless the user explicitly asks for history.
- If sources are only low-authority or stale, mark the result as risk or block;
  do not present them as sufficient context.
