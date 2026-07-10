---
status: active
authority: true
layer: authority
type: workflow
source_of_truth: code
supersedes: []
superseded_by: []
owner: user
last_verified: 2026-07-08
---

# Development Workflow

The development workflow is owned by the current skill implementation under
`skills/development/`, the shared protocol, and the assessment skills for
read-only review and manual challenge behavior.

## Skill Ownership

| Area | Owner |
|---|---|
| Case identity, delayed case creation, status-only routing, artifact policy | `docloom-workflow` |
| Minimal context and authority checks before planning | `context-authority` |
| Plan generation, risk, version, base commit, and user confirmation | `plan-confirm` |
| TDD execution, execution evidence, semantic task/fix commits, Post-execution invocation and fix loop | `tdd-execute` |
| Closure, closure commit, L2/L3 sync, and authority proposals or confirmed narrow patches | `doc-sync-close` |
| Read-only ad-hoc and workflow-owned Post-execution evidence review | `review` |
| Interactive pressure testing | `grill` |

`docloom-workflow` is a thin router. It does not replace stage skills, generate
plans, execute implementation work, auto-trigger ad-hoc review, or call a CLI
backend. Mandatory Post-execution review is invoked inside `tdd-execute`, not
routed as a new phase.

`context-authority` is conditional. Use it for resume, case ambiguity,
high-risk or public-contract work, workflow or agent policy, or conflicts; do
not make it a default phase for low-risk one-step work.

When reading project authority, `context-authority` reads the declared active
constitution first when it is discoverable. Missing a conventional constitution
file is not by itself missing authority.

`plan-confirm` consumes case identity created elsewhere. It writes `plan.md` as
`status: draft`, records risk and TDD strategy, and asks for confirmation.
Confirming the current plan authorizes same-turn execution unless the user asks
to hold, revise, or review first.

`tdd-execute` requires an approved plan plus current execution authorization:
same-turn current-plan approval, current execute/continue intent, valid
Fast-Path, or reconfirmation of an older approved plan. TDD is required by
default; exceptions must be recorded in the plan or a confirmed same-turn
amendment and must include alternative verification.

For an eligible persistent case, `tdd-execute` creates semantic atomic commits
at independently valid green task, verified refactor, and material review-fix
boundaries. After planned execution and verification, it invokes `review` in
Completed Post-execution mode, persists separate Engineering and Spec results,
and owns the fix/re-review loop until the aggregate result passes.

`doc-sync-close` writes `closure.md` before updating case state to closed. It
may mechanically sync safe derived docs, but authority changes are proposals by
default unless a concrete narrow patch is confirmed. For plans declaring the
atomic policy, it creates the closure commit and reports unqualified `Done` only
after Git proves that commit succeeded and the case-related worktree is clean.

`docs/cases/README.md` may exist as a derived case dashboard for discovery. It
does not replace per-case routing truth or evidence; case artifacts remain the
source of truth.

`docloom-workflow` may perform next-slice discovery from
`docs/product/current-state.md` or equivalent user-provided facts. This produces
ranked recommendations only; selected candidates still enter the normal
`plan-confirm` confirmation flow before execution.

Ad-hoc `review` and `grill` are manual helpers. `review` also owns the
workflow-authorized read-only Post-execution mode. Neither writes files, updates
case state, routes workflow, or creates authority/governance proposals.

## Fact Authority

Use the shared fact authority order:

1. Active authority docs.
2. Current production code or skill implementation.
3. Current tests.
4. Accepted ADR, migration note, or release note.
5. User-provided new information in the current turn, as pending fact.
6. L2 operational case docs.
7. L3 derived or index docs.
8. L4 archive or historical docs.
9. L5 scratch docs.

Generated views can be consumed, but they cannot override their source.
Historical material is evidence, not current authority.

## Confirmation Policy

Low-risk work follows the smallest applicable path:

- One-shot low-risk local changes do not create case docs, plans, execution
  reports, or closure records.
- Fast-Path persistent cases may use `approved_by: fast-path` when all
  Fast-Path conditions in shared protocol are verified and recorded in the
  minimal plan.
- Non-Fast-Path low-risk and medium-risk persistent work requires user
  confirmation of the current plan; confirmation authorizes same-turn execution
  unless the user asks to hold.
- High-risk work requires explicit confirmation of the current object; same-turn
  execution follows unless the user asks to hold.

Current-plan approval also authorizes only the case-scoped semantic commits
declared by its Atomic Commit Strategy. It does not authorize unrelated files,
push, publication, history rewriting, material deviations, or unlisted
dependency, CI, schema, config-contract, or authority work.

`approved_by: fast-path` means the agent verified the Fast-Path conditions
under the current user request. It does not authorize unrelated background
execution.

Approved plans are not standing execution grants. Later status, resume, or
dashboard discovery still needs current execute, continue, or reconfirm intent.

## Post-Execution Quality Gate

Eligible persistent cases that change code, tests, Skill behavior, workflow
policy, public contracts, executable configuration, or user-observable behavior
must complete a read-only Post-execution review before successful closure. The
approved plan supplies the exact baseline and authorizes the gate without a
separate reminder.

The review covers the complete case delta, including committed, staged,
unstaged, and untracked case changes. Engineering checks technical soundness,
tests, contracts, standards, triggered high-risk concerns, Git evidence, and
unnecessary complexity. Spec checks the approved Goal, Non-goals, Decisions,
Acceptance Criteria, amendments, missing behavior, incorrect behavior, and
scope creep. The axes remain separate; one cannot compensate for the other.

Critical or Important findings return to execution for the smallest verified
root-cause fix, an atomic fix commit, and re-review. Missing material evidence
cannot pass. No new phase or mandatory `review.md` artifact is introduced;
durable evidence lives in `execution.md` and `closure.md`.

## Semantic Atomic Commits

For plans declaring the policy, approved requirements when present, approved
plan, independently valid green tasks, verified refactors, material review
fixes, and closure are semantic commit boundaries. Atomic means one coherent,
verified, independently reviewable and revertible unit, not one commit per
checkbox. Red states, failed attempts, empty stages, and timestamps are not
normal completion commits.

Each case commit follows the Git authority, excludes unrelated changes, leaves
relevant checks passing, and maps to the case and completion point. Fast-Path
may use one compact implementation commit, but still performs Engineering/Spec
review and a closure commit. These rules are prospective for plans that declare
them and do not invalidate legacy cases.

## Sources

- `skills/_shared/references/shared-protocol.md`
- `skills/development/docloom-workflow/SKILL.md`
- `skills/development/context-authority/SKILL.md`
- `skills/development/context-authority/references/retrieval-routing.md`
- `skills/development/plan-confirm/SKILL.md`
- `skills/development/plan-confirm/references/risk-levels.md`
- `skills/development/tdd-execute/SKILL.md`
- `skills/development/doc-sync-close/SKILL.md`
- `skills/_shared/references/loop-protocol.md`
- `docs/product/current-state.md`
- `skills/assessment/review/SKILL.md`
- `skills/assessment/grill/SKILL.md`
- `docs/cases/README.md`
