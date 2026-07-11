---
status: active
authority: true
layer: authority
type: workflow
source_of_truth: code
supersedes: []
superseded_by: []
owner: user
last_verified: 2026-07-12
---

# Development Workflow

Current Skill implementation owns workflow procedure. Shared ownership,
authority order, artifacts, status, authorization, compatibility, and Fast-Path
rules live in `skills/_shared/references/shared-protocol.md`.

## Stage Contract

- `docloom-workflow` is the human-facing doorway, thin router, and case-identity
  owner. It routes stage ownership internally and does not plan, execute,
  auto-trigger ad-hoc review, or call a backend.
- `context-authority` is a conditional pre-plan gate for resume, ambiguity,
  authority, conflict, public-contract, and high-risk work. It reads the active
  constitution first when discoverable.
- `plan-confirm` writes and confirms the current versioned plan. It owns
  approved requirements and plan commits; current approval allows same-turn
  execution unless the user asks to hold, revise, or review first.
- `tdd-execute` requires current execution authorization, defaults to TDD, and
  records confirmed exceptions with alternative verification.
- `doc-sync-close` owns closure, safe L2/L3 sync, and confirmed narrow authority
  patches; structural authority work remains governance work.
- `review` is read-only and supports explicit ad-hoc assessment plus the
  workflow-owned Post-execution gate. `grill` remains explicit and manual.
- `docs/cases/README.md` and `docs/product/current-state.md` are derived inputs,
  never routing or authority truth. Discovery recommendations still require
  normal case identity, planning, and approval.

Case status is owned by existing artifacts: plan approval in `plan.md`,
execution readiness in `execution.md`, and final outcome in `closure.md`. There
is no separate routing-state artifact; legacy state files are historical
evidence only.

## Confirmation

Use the smallest path: one-shot low-risk work needs no case; verified Fast-Path
cases may use `approved_by: fast-path`; other persistent work requires current
plan confirmation, and high-risk work requires an unambiguous current object.

Current-plan approval authorizes only its declared case-scoped semantic commits.
It excludes publication, history rewriting, material deviations, unrelated
changes, and unlisted high-risk scope; see shared protocol for the full boundary.
An approved plan is not a standing grant for a later session.

The confirmation surface must summarize the human outcome, material scope,
expected local Git actions/commit count, likely interruptions, and excluded
publication/history actions before asking for approval.

## Required Quality Outcomes

Eligible persistent cases must complete Post-execution review before successful
closure. `tdd-execute` invokes `review`; Engineering and Spec remain separate,
missing material evidence cannot pass, and material findings return to a
verified atomic fix and re-review. This adds no case phase or `review.md`.

Full plans declaring atomic commits use semantic completion points: approved
requirements when present, approved plan, independently valid green work,
verified refactors, material review fixes, and closure. Fast-Path still performs
a compact Engineering/Spec review but uses no separate plan/task/closure
commits: the approved minimal plan, green delta, review evidence, closure, and
final status form one combined completion commit. The policy is prospective and
does not invalidate legacy cases.

Unqualified `Done` requires met acceptance criteria, passing required review,
a successful declared full-flow closure or Fast-Path completion commit, and no
unexplained case-related worktree changes.

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
