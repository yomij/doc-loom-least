---
case_id: 20260807-outcome-only-plan-contract
plan_version: 1
status: approved
risk_level: medium
assurance_mode: guarded
approved_by: user
approved_at: 2026-08-07T10:28:50+08:00
confirmation: 'User said “执行”, explicitly authorizing current plan v1.'
base_commit: e7910db2a8c35560d456640ec0222f9fdab74697
final_status:
closed_at:
---

# Goal

Make `plan.md` a durable outcome contract that tells an agent what result is
authorized, how completion will be judged, and which limits must not be crossed,
while leaving implementation-path discovery and adaptation to the executing
agent.

# Success Criteria

| Criterion | Evidence required | Status | Evidence |
|---|---|---|---|
| A current plan's human-facing body contains exactly Goal, Success Criteria, and Constraints; lifecycle metadata remains concise frontmatter rather than a fourth planning concern. | The canonical template and active contracts expose only those three body sections. | pending | |
| Goal states the desired end condition and purpose without prescribing files, tasks, commands, sequencing, or a solution shape. | Template guidance and Skill rules reject implementation-path content in Goal. | pending | |
| Success Criteria make “done” observable and falsifiable, name the evidence needed to support each claim, and can be updated with final status/evidence at Compact close. | The template supports criterion, evidence requirement, status, and actual evidence without adding a final-summary section. | pending | |
| Constraints carry guardrails, non-goals, protected authority or compatibility effects, reapproval triggers, excluded actions, and any required assurance; they do not prescribe an implementation path unless the constraint itself is owner-mandated. | Active authorization and deviation rules map to Constraints and preserve all current stop/reconfirm conditions. | pending | |
| Context gathering, risk classification, file discovery, task decomposition, TDD/alternative-verification choice, review invocation, and commit organization remain agent responsibilities outside the plan body. | Ownership across `context-authority`, `plan-confirm`, `tdd-execute`, `review`, and `doc-sync-close` is unambiguous and non-duplicative. | pending | |
| Compact and Guarded paths retain current authorization strength, exact-baseline review where triggered, path-correct terminal status, and evidence-backed Done gates. | Shared protocol, authority, executor/reviewer/closer contracts, and templates agree; guarded Post-execution Engineering/Spec review passes. | pending | |
| Existing historical Case artifacts remain valid without migration, and active explanatory documentation no longer describes the superseded plan shape or the stale Compact `plan + closure` model. | Semantic audit finds no conflicting active contract; historical Case files remain untouched. | pending | |
| Changed Skill packages and Markdown/YAML structures validate with the repository's available validators. | Structural validation and targeted semantic checks pass with no unexplained working-tree changes. | pending | |

# Constraints

- Preserve the existing Case stages, Direct/Compact/Guarded proportional paths,
  authority order, approval versioning, and same-turn execution behavior.
- Plan authorization must remain bound to the approved Goal, Success Criteria,
  and Constraints. A change to any of them, a risk escalation, an
  authority/public-contract effect, a dependency/lockfile/CI/schema/config
  effect, an external resource, an irreversible action, or another protected
  effect requires a new draft version and confirmation.
- Keep identity, version, risk/assurance, approval, exact baseline, and terminal
  status as minimal frontmatter metadata; do not create another state artifact
  or backend.
- Do not persist implementation files, task lists, commands, sequencing, run
  mode, TDD strategy, review strategy, commit strategy, or speculative context
  in `plan.md`.
- A confirmed TDD exception or owner-mandated implementation restriction is a
  Constraint; ordinary verification and implementation choices belong to
  execution. For this pure Skill/document contract change, do not invent a
  meaningless Red; completion still requires structural, semantic, diff-scope,
  and guarded review evidence.
- Guarded confirmation may summarize likely work and local Git effects in the
  conversation, but must not expand the persisted plan interface.
- Do not weaken authorization, perform unrelated cleanup, migrate historical
  Cases, publish, push, rewrite history, or create a local commit unless the user
  separately requests it.
