---
status: active
authority: true
layer: authority
type: workflow
source_of_truth: code
supersedes: []
superseded_by: []
owner: user
last_verified: 2026-08-06
---

# Development Workflow

Current Skill implementation owns workflow procedure. The shared protocol is a
compact kernel for cross-skill ownership, authority, status, artifacts,
authorization, compatibility, and proportional-assurance invariants. Stage
procedure stays with its owner; detailed references and templates load only
when triggered.

## Stage Contract

- `docloom-workflow` is the human-facing doorway, thin router, and case-identity
  owner. It routes stage ownership internally and does not plan, execute,
  auto-trigger ad-hoc review, or call a backend.
- `context-authority` is a conditional pre-plan gate for resume, ambiguity,
  authority, conflict, public-contract, high-risk, and weakly verified work. It
  reads the active constitution first when discoverable.
- `plan-confirm` writes and confirms the current versioned plan. It owns the
  outcome envelope and any declared approval/plan commit evidence; current
  authorization allows same-turn execution unless the user asks to hold,
  revise, or review first.
- `tdd-execute` requires current execution authorization, defaults to TDD, and
  records confirmed exceptions with alternative verification.
- `doc-sync-close` owns path-correct terminal status, safe L2/L3 sync, and
  confirmed narrow authority patches; structural authority work remains
  governance work.
- `review` is read-only and supports explicit ad-hoc assessment plus the
  workflow-owned Post-execution gate. `grill` remains explicit and manual.
- `docs/cases/README.md` and `docs/product/current-state.md` are derived inputs,
  never routing or authority truth. After candidate selection, apply the same
  two questions; direct work does not acquire case ceremony merely because it
  came from discovery.

Case status is owned by existing artifacts: plan approval in `plan.md`,
execution readiness in `execution.md` when that artifact is triggered, and
final outcome either in thin `closure.md` (Guarded or legacy) or in Compact
plan terminal fields (`final_status`, `closed_at`). There is no separate
routing-state artifact; legacy state files are historical evidence only.

## Proportional Paths And Confirmation

Answer two independent questions: whether continuity, a durable decision,
explicit case request, or guarded execution requires a case; and whether
consequence, irreversibility, exposure, authority impact, or weak verification
requires guarded assurance.

- Direct reversible one-turn low/medium work uses normal execution,
  verification, and final reporting without a case.
- Compact persistent work uses a concise plan and records terminal status on
  that plan when the case ends; the current unambiguous execute request may be
  recorded as approval without another prompt. Compact does not require a
  separate `closure.md`.
- Guarded work requires explicit confirmation of the written current plan,
  exact-baseline deep review, and thin `closure.md` terminal evidence.

Medium risk alone does not create a case or trigger confirmation.
All paths retain applicable TDD or a credible recorded alternative verification.

Approval binds Goal, Guardrails/Non-goals, Acceptance, Escalation Triggers, and
specifically protected boundaries. Internal file discovery, added tests, and
ordinary implementation choices may adapt within that outcome envelope.
Publication, history rewriting, unrelated changes, risk escalation,
authority/public-contract changes, dependency/config/CI/schema effects,
external resources, irreversible actions, and other declared protected
boundaries require separate authorization. An approved plan is not a standing
grant for a later session.

Before guarded confirmation, summarize the human outcome, material scope,
expected local Git actions/commit count, likely interruptions, and excluded
publication/history actions.

## Required Quality Outcomes

Deep Post-execution Engineering/Spec review is required for guarded work,
material deviations, weak verification, public/authority-sensitive changes,
or an explicit plan/user request. `tdd-execute` invokes `review`; axes remain
separate and missing material evidence cannot pass. Other work receives the
executor's compact acceptance/test/diff/scope completion check. This adds no
case phase or `review.md`.

Review returns the complete current finding set. Execution fixes findings in
the smallest coherent, independently valid/revertible batches and re-reviews
affected axes; finding count does not dictate commit count.

Local commits follow user/project intent and semantic value. Ordinary case
bookkeeping does not require standalone plan or closure commits. Guarded plans
may declare them when auditability justifies the cost. The policy is prospective
and does not invalidate legacy cases.

Unqualified `Done` requires met acceptance criteria, passing review when
triggered, complete path-correct terminal evidence (Compact plan fields or
Guarded thin `closure.md`), and no unexplained case-related worktree changes.
Commit success is an additional terminal gate only when the approved plan
declared that commit.

## Sources

- `skills/_shared/references/shared-protocol.md`
- `skills/development/docloom-workflow/SKILL.md`
- `skills/development/context-authority/SKILL.md`
- `skills/development/plan-confirm/SKILL.md`
- `skills/development/plan-confirm/references/tdd-exceptions.md`
- `skills/development/tdd-execute/SKILL.md`
- `skills/development/doc-sync-close/SKILL.md`
- `skills/development/docloom-workflow/references/loop-protocol.md`
- `docs/product/current-state.md`
- `skills/assessment/review/SKILL.md`
- `skills/assessment/grill/SKILL.md`
- `docs/cases/README.md`
