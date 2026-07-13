---
name: context-authority
description: Conditional pre-plan context and authority check for resume, case ambiguity, conflicts, public/high-risk work, workflow/agent policy, or missing planning context. Skip explanations and routine low-risk work.
---

# context-authority

Read the smallest evidence needed for a planning verdict. Do not plan, edit,
create case identity, or resolve authority conflicts.

Read `references/shared-protocol.md` only for shared authority, identity,
status, risk, and resume rules. Read
`templates/context-authority-brief.md` only when persisting a real brief.

## Start

Reuse the router's workspace snapshot or run the three minimal Git checks. Read
changed paths only for dirtiness, resume, case matching, or the coming baseline.
Without Git, continue context work and record `git_available: false`.

## Use Boundary

Run before planning when resume/identity is ambiguous, authority or evidence may
conflict, the task touches public/high-risk or ADR boundaries, workflow/agent
policy is changing, or planning lacks safe context. Skip clear one-step work,
mechanical docs, explanations, and work already supported by an inline context
summary or valid low-risk skip.

Governance initialization belongs to `setup-doc-governance`. Ad-hoc review
still requires explicit user intent.

## Intent And Minimum Evidence

| Intent | Minimum evidence |
|---|---|
| Bug | Symptom/reproduction, related entry, verification path. |
| Feature | Goal/acceptance, relevant authority/contract or explicit absence, adjacent implementation. |
| Refactor | Target boundary, verification coverage, non-touch scope. |
| Docs governance | Scope, authority/index state, blocked decisions/history needed. |
| Workflow/Skill | Target owner, shared/adjacent contracts, constitution/authority constraints. |
| Resume | Current artifacts, handoff when present, Git state. |
| Incident | Symptom, impact, rollback/runbook evidence. |

Record the chosen intent and exclude unrelated material. Follow shared case
identity and terminal/resume rules. Select `isolated`, `branch`, or `inline`,
but create no branch, worktree, case, or artifact.

## Workflow

1. Classify intent and workspace.
2. Resolve an existing case or propose a slug; never create it.
3. Read active constitution/authority/governance by relevance.
4. Read case artifacts only for explicit/resumed cases.
5. Read direct code/test paths only when the intent needs them.
6. Record included/excluded sources, trust, conflict, and risk.
7. Return one verdict.

## Evidence Routing

For governance, authority, workflow/agent policy, public contract, or high-risk
work, read the active constitution and authority index first. Then use the
active case governance plan, newest approved or applied-with-blocks governance
plan, and legacy governance only as context. Use only active authority facts;
exclude archived, superseded, and generated views. Missing governance permits
low-risk local planning with recorded risk, while public/high-risk work may
require governance or a user decision.

Read `plan.md`, `handoff.md`, `execution.md`, and `closure.md` only for an
explicit or resumed case; legacy state files are diagnostic only. Find code and
tests through user identifiers, authority paths, and exact `rg` searches.
Runtime/log evidence needs a time window and trust note.

Record the case or proposed slug, intent, risk, every included source with its
layer/reason/trust, and every excluded source with its reason. Derived or
historical evidence stays labelled.

## Verdict

| Verdict | Use |
|---|---|
| `proceed_to_plan` | Sufficient context; no blocking conflict. |
| `proceed_to_plan_with_risk` | Planning may continue with weak/missing/non-blocking evidence recorded. |
| `needs_user_decision` | A material fact or choice is missing. |
| `needs_case_selection` | Multiple cases exist with no safe choice. |
| `run_setup_doc_governance` | Authority/governance must be established first. |
| `blocked_by_authority_conflict` | Active authority, implementation, tests, or confirmed facts conflict materially. |

Default output is an inline summary for `plan-confirm`. Persist
`context-authority-brief.md` only for conflict, explicit request, continuity, or
context too large for the plan; high risk/resume alone does not require it.

Blocking conflicts include high-risk authority disagreement, recent
owner-confirmed fact conflict, code/test disagreement, material missing
coverage, or unconfirmed workflow/agent-policy change. Low-authority internal
drift may proceed only when reversible, non-public, and recorded as risk.

## Gates

- No usable context -> decision/case-selection verdict, not a plan.
- Blocking conflict -> no planning.
- Missing identity -> return the proposed slug to `docloom-workflow`.
- Archived, superseded, generated, stale, or scratch sources are not current
  facts unless history is requested.
- Weak/stale-only evidence must remain an explicit risk or blocker.
