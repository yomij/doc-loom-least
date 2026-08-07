---
name: context-authority
description: Conditional pre-plan context/authority check for resume, case ambiguity, conflict, guarded/public/authority-sensitive work, workflow/agent policy, weak verification, or missing context. Skip explanations and routine direct work.
---

# context-authority

Read the least evidence needed for a planning verdict. Do not plan, edit,
create case identity, or resolve authority conflict.

Read when trigger condition is met:

- [Shared protocol](./references/shared-protocol.md): shared authority,
  identity, status, risk, or resume rules only.
- [Context-authority brief template](./templates/context-authority-brief.md):
  persisting a real brief only.

## Start

Reuse the router snapshot or run its three Git checks. Inspect changed paths
only for dirtiness, resume, case match, or baseline. Without Git, continue and
record `git_available: false`.

## Use Boundary

Run before planning for ambiguous resume/identity, possible authority/evidence
conflict, guarded or public/ADR work, workflow/agent-policy change, weak
verification, or insufficient safe context. Skip direct one-turn work,
mechanical docs, explanations, and work with an inline summary or valid skip.
Medium risk alone is not a trigger.

Governance initialization belongs to `setup-doc-governance`. Ad-hoc review
still requires explicit user intent.

## Intent And Minimum Evidence

| Intent | Minimum evidence |
|---|---|
| Bug | Symptom/reproduction, related entry, verification path. |
| Feature | Goal/success conditions, relevant authority/contract or explicit absence, adjacent implementation. |
| Refactor | Target boundary, verification coverage, non-touch scope. |
| Docs governance | Scope, authority/index state, blocked decisions/history needed. |
| Workflow/Skill | Target owner, shared/adjacent contracts, constitution/authority constraints. |
| Resume | Current artifacts, handoff when present, Git state. |
| Incident | Symptom, impact, rollback/runbook evidence. |

Record intent and exclude unrelated material. Follow shared identity and
terminal/resume rules. Select `isolated`, `branch`, or `inline` without creating
a branch, worktree, case, or artifact.

## Workflow

Classify intent/workspace; resolve an existing case or propose a slug; read
relevant active constitution/authority/governance; read artifacts only for an
explicit/resumed case and code/tests only when needed; record source inclusion,
exclusion, trust, conflict, and risk; return one verdict. Never create the case.

## Evidence Routing

For governance, authority, workflow/agent policy, public contract, or high-risk
work, read the active constitution and authority index first; then the active
case governance plan, newest approved/applied-with-blocks governance plan, and
legacy governance as context only. Exclude archived, superseded, and generated
views from current facts. Missing governance permits recorded-risk local
low-risk planning; public/high-risk work may need governance or user decision.

For an explicit/resumed case, read present `plan.md` (including Compact terminal
fields), `handoff.md`, `execution.md`, and `closure.md`; legacy state is
diagnostic. Find code/tests through user identifiers, authority paths, and exact
`rg`. Runtime/log evidence needs a time window and trust note.

Record case/proposed slug, intent, risk, included sources with layer/reason/
trust, and exclusions with reasons. Label derived/historical evidence.

## Verdict

| Verdict | Use |
|---|---|
| `proceed_to_plan` | Sufficient context; no blocking conflict. |
| `proceed_to_plan_with_risk` | Planning may continue with weak/missing/non-blocking evidence recorded. |
| `needs_user_decision` | A material fact or choice is missing. |
| `needs_case_selection` | Multiple cases exist with no safe choice. |
| `run_setup_doc_governance` | Authority/governance must be established first. |
| `blocked_by_authority_conflict` | Active authority, implementation, tests, or confirmed facts conflict materially. |

Default to an inline summary for `plan-confirm`, not plan-body copy. Persist
`context-authority-brief.md` only for conflict, explicit request, continuity, or
independently durable support; high risk/resume alone does not require it.

Block on high-risk authority disagreement, recent owner-confirmed fact conflict,
code/test disagreement, material coverage gaps, or unconfirmed workflow/agent-
policy change. Low-authority internal drift may proceed only if reversible,
non-public, and recorded as risk.

## Gates

- No usable context -> decision/case-selection verdict, not a plan.
- Blocking conflict -> no planning.
- Missing identity -> return the proposed slug to `docloom-workflow`.
- Archived, superseded, generated, stale, or scratch sources are not current
  facts unless history is requested.
- Weak/stale-only evidence must remain an explicit risk or blocker.
