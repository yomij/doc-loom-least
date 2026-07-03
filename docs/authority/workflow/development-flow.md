---
status: active
authority: true
layer: authority
type: workflow
source_of_truth: code
supersedes: []
superseded_by: []
owner: user
last_verified: 2026-07-02
---

# Development Workflow

The development workflow is owned by the current skill implementation under
`skills/development/`, the shared protocol, and the assessment skills for
manual review/challenge behavior.

## Skill Ownership

| Area | Owner |
|---|---|
| Case identity, delayed case creation, status-only routing, artifact policy | `docloom-workflow` |
| Minimal context and authority checks before planning | `context-authority` |
| Plan generation, risk, version, base commit, and user confirmation | `plan-confirm` |
| TDD execution, execution evidence, plan progress, and review-risk signal | `tdd-execute` |
| Closure, L2/L3 sync, and authority proposals or confirmed narrow patches | `doc-sync-close` |
| Read-only evidence review | `review` |
| Interactive pressure testing | `grill` |

`docloom-workflow` is a thin router. It does not replace stage skills, generate
plans, execute implementation work, auto-trigger review, or call a CLI backend.

`context-authority` is conditional. Use it for resume, case ambiguity,
high-risk or public-contract work, workflow or agent policy, or conflicts; do
not make it a default phase for low-risk one-step work.

When reading project authority, `context-authority` reads the declared active
constitution first when it is discoverable. Missing a conventional constitution
file is not by itself missing authority.

`plan-confirm` consumes case identity created elsewhere. It writes `plan.md` as
`status: draft`, records risk and TDD strategy, and waits for confirmation
before persistent execution unless the case meets all Fast-Path conditions.

`tdd-execute` executes only an approved case plan. TDD is required by default;
exceptions must be recorded in the plan or a confirmed same-turn amendment and
must include alternative verification.

`doc-sync-close` writes `closure.md` before updating case state to closed. It
may mechanically sync safe derived docs, but authority changes are proposals by
default unless a concrete narrow patch is confirmed.

`docs/cases/README.md` may exist as a derived case dashboard for discovery. It
does not replace per-case routing truth or evidence; case artifacts remain the
source of truth.

`docloom-workflow` may perform next-slice discovery from
`docs/product/current-state.md` or equivalent user-provided facts. This produces
ranked recommendations only; selected candidates still enter the normal
`plan-confirm` confirmation flow before execution.

`review` and `grill` are manual helpers. They do not write files, update case
state, route workflow, or create authority/governance proposals.

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
  confirmation before execution.
- High-risk work requires explicit confirmation of the current object.

`approved_by: fast-path` means the agent verified the Fast-Path conditions
under the current user request. It does not authorize unrelated background
execution.

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
