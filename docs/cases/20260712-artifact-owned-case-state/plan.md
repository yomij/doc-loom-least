---
case_id: 20260712-artifact-owned-case-state
plan_version: 2
status: approved
risk_level: high
approved_by: user
approved_at: 2026-07-12T12:41:15+08:00
base_commit: 9e6c48c4bc3eba04162cfb121d593ea42287e06e
---

# Implementation Plan

## Human Approval Summary

- Outcome: remove `case_state.yaml` as a second routing truth and let the
  existing case artifacts carry their own current status.
- Material scope: shared workflow protocol, five development/assessment skill
  consumers, three artifact templates, workflow authority, derived workflow
  explanations, and this case's migration evidence.
- Decision needed: approve amended plan v2, which adds the three files required
  to resolve independent review findings and clarifies closure-owned acceptance.
- Expected local Git actions / commit count: the branch, v1 plan commit, and
  implementation commit already exist. After v2 approval, create three more
  local commits: approved plan amendment, one coherent review fix, and closure.
- Excluded actions: no push, PR, merge, tag, release, amend, rebase, squash,
  history rewrite, dependency change, new runtime component, new skill, or
  historical-case rewrite.

## Goal

Make Markdown artifacts the single current source for case lifecycle status:

1. New cases no longer create `case_state.yaml`.
2. `plan.md`, `execution.md`, and `closure.md` carry the status owned by their
   stage.
3. Status-only routing derives the current position deterministically from the
   newest authoritative artifact, without cache repair or phase-writer rules.
4. Remove routing fields that merely repeat conversational output, including
   `next_skill`, `route_reason`, `required_input`, `case_docs`, and
   `closure_path`.
5. Preserve approval, review, evidence, Git authorization, Fast-Path, and
   historical case compatibility while shrinking the active contract.

## Non-goals

- Do not merge `execution.md` and `closure.md` in this case.
- Do not remove or rename Fast-Path, Post-execution review, risk levels, skills,
  or closure status values.
- Do not weaken user confirmation, public-contract, authority, or Git safety
  boundaries.
- Do not rewrite closed historical case directories or delete their old state
  files.
- Do not change the Constitution, add an artifact type, or introduce a runtime
  state engine.

## Context

- Intent: workflow / skill design.
- Route Verdict: `proceed_to_plan`.
- Proposed case slug: `20260712-artifact-owned-case-state`.
- User request: begin implementing the preceding complexity review.
- Selected first slice: remove the dual state/artifact truth before addressing
  later artifact merging or generic prose deletion.
- Active cases before creation: none.
- Workspace: clean `main` at the exact baseline in frontmatter.
- Authority fit: the Constitution requires the minimum path, fewer moving
  parts, and no duplicate artifacts or ceremony. Current skills are the
  implementation source for workflow procedure.

Context sources:

| Source | Why Included | Trust |
|---|---|---|
| `docs/authority/constitution.md` | Minimum-path and human-first constraints. | Highest |
| `docs/authority/README.md` | Current authority order. | High |
| `docs/authority/workflow/development-flow.md` | Active workflow contract and owners. | High |
| `docs/authority/agent/policy.md` | Workflow-policy risk and agent responsibility. | High |
| `skills/_shared/references/shared-protocol.md` | Current state, artifact, routing, authorization, and compatibility implementation. | High |
| Current stage Skills and templates | Observable consumers of case state. | High |
| `docs/cases/README.md` and derived workflow docs | Current user/agent navigation claims that must remain aligned. | Derived |
| Closed recent simplification cases | Confirms earlier work deliberately retained state and did not complete this slice. | Historical evidence |

Excluded context:

- Archived designs: active authority and current skill implementation are
  sufficient; historical proposals cannot override them.
- Runtime code/tests: the repository has no workflow interpreter or test suite.
- Other closed case bodies: they will not be migrated and are unnecessary for
  defining the prospective contract.

## Decisions

1. `plan.md` keeps `status: draft | approved` as plan/approval truth.
2. `execution.md` gains frontmatter with `case_id` and
   `status: executing | ready_to_close`.
3. `closure.md` gains frontmatter with `case_id`, final `status`, and
   `updated_at`; its existing final status set remains unchanged.
4. Route derivation order is `closure.md` -> `execution.md` -> `plan.md` ->
   insufficient context. Git evidence may qualify whether a final status is
   fully complete, but it does not create a parallel phase field.
5. `docloom-workflow` creates only the case directory; `plan-confirm` writes the
   first authoritative artifact in the same flow.
6. `next_skill`, route reason, and required input remain human-facing output or
   handoff content, not durable state fields.
7. Existing `case_state.yaml` files remain untouched historical evidence. When
   inspecting a legacy case, current artifacts take precedence and the state
   file is diagnostic only.
8. This case is created under the old protocol but deletes its own state file
   during implementation and closes under the new artifact-owned contract.
9. `Done`, `Cancelled`, `Superseded`, and `Abandoned` closures are terminal.
   `Paused`, `Blocked`, and `Done with Caveats` may resume when an explicitly
   resumed `execution.md` has a later `updated_at` than `closure.md`; the newer
   execution artifact then owns current status until the next closure.
10. `handoff.md` records resume conditions and source artifacts, not duplicated
    current phase or closure status.
11. Post-execution review judges implementation readiness. A criterion requiring
    `closure.md` and its commit remains pending until `doc-sync-close`; it is a
    Done Gate condition, not evidence that must exist before review.

## Workspace Baseline

- Base Commit: `9e6c48c4bc3eba04162cfb121d593ea42287e06e`
- Branch Before Planning: `main`
- Planned Run Mode: `branch`
- Planned Branch: `codex/artifact-owned-case-state`
- Dirty Workspace Before Case: no
- Existing Changed Files Before Case: none

## Risk Level

High under the current policy because this changes active workflow routing
truth and the L1 workflow contract. The change is reversible Markdown and does
not weaken authorization, but a routing mistake could misclassify case status
or allow premature closure.

## TDD Applicability

- TDD Required: No.
- Exception: pure Markdown Skill, template, authority, and derived-doc changes;
  no runtime workflow interpreter exists.
- Characterization lock: capture every active `case_state.yaml`, phase writer,
  cache conflict, state repair, and routing-field reference before editing.
- Alternative Verification:
  - semantic positive and stale-rule `rg` assertions;
  - YAML/frontmatter parsing for all active templates and case artifacts;
  - `uv run --with pyyaml` Skill validation for every modified Skill;
  - link and symlink validation;
  - `git diff --check` and complete-delta inspection;
  - mandatory isolated Engineering and Spec Post-execution review.

## Post-Execution Review Strategy

- Exact baseline: approved `base_commit` above.
- Engineering axis: verify deterministic artifact-derived routing, template
  validity, legacy compatibility, Fast-Path/full-flow behavior, clean Git
  scope, and absence of a new hidden state mechanism.
- Spec axis: compare the final delta with Goal, Non-goals, Decisions,
  Acceptance Criteria, the complexity-review conclusion, Constitution, and
  active workflow/agent authority.
- Critical/Important findings return to one smallest coherent `fix:` commit and
  affected-axis re-review. Missing material evidence cannot pass.
- Durable review evidence: `execution.md` and `closure.md`; no `review.md`.

## Atomic Commit Strategy

| Step | Commit Title | Trailer |
|---|---|---|
| Approved plan | `docs: approve artifact-owned case state plan` | `Doc-Loom-Step: plan` |
| Coherent implementation | `refactor: derive case state from artifacts` | `Doc-Loom-Step: task:artifact-state` |
| Approved plan amendment | `docs: approve artifact-owned case state plan v2` | `Doc-Loom-Step: plan-amendment` |
| Review fix | `fix: complete artifact-owned status semantics` | `Doc-Loom-Step: review-fix:F1-F4` |
| Closure | `docs: close artifact-owned case state case` | `Doc-Loom-Step: closure` |

All commits use `Doc-Loom-Case: 20260712-artifact-owned-case-state`, explicit
path staging, staged-diff inspection, `git diff --cached --check`, and relevant
validation. Approval authorizes only these local commits and planned branch
creation. It does not authorize publication or history rewriting.

## Files to Change

Core contract and routing:

- `skills/_shared/references/shared-protocol.md`
- `skills/_shared/references/loop-protocol.md`
- `skills/development/docloom-workflow/SKILL.md`
- delete `skills/development/docloom-workflow/templates/case_state.yaml`
- `skills/development/context-authority/references/retrieval-routing.md`
- `skills/development/context-authority/references/context-resolution.md`
- `skills/development/plan-confirm/SKILL.md`
- `skills/development/tdd-execute/SKILL.md`
- `skills/development/doc-sync-close/SKILL.md`
- `skills/assessment/review/SKILL.md`

Artifact templates:

- `skills/development/plan-confirm/templates/plan.md`
- `skills/development/tdd-execute/templates/execution.md`
- `skills/development/doc-sync-close/templates/closure.md`
- `skills/_shared/templates/handoff.md`

Authority and derived explanations:

- `docs/authority/workflow/development-flow.md`
- `docs/authority/operations/git.md`
- `docs/ssot-map.md`
- `docs/share/workflow-and-design.md`
- `docs/cases/README.md`

Case evidence:

- `docs/cases/20260712-artifact-owned-case-state/case_state.yaml` (delete during implementation)
- `docs/cases/20260712-artifact-owned-case-state/plan.md`
- create `docs/cases/20260712-artifact-owned-case-state/execution.md`
- create `docs/cases/20260712-artifact-owned-case-state/closure.md`

## Acceptance Criteria

| Criteria | Planned Verification |
|---|---|
| New case creation does not create `case_state.yaml`. | Inspect router contract/template tree and stale-reference assertions. |
| Plan, execution, and closure artifacts each carry only the status owned by that stage. | Parse templates and this case artifacts; inspect ownership text. |
| Status-only routing follows one documented artifact precedence without cache repair or phase writers. | Semantic assertions across shared/router/context/loop contracts. |
| Active contracts contain no `next_skill`, `route_reason`, `required_input`, `case_docs`, or `closure_path` routing fields. | Targeted stale-rule `rg`, excluding historical cases. |
| Fast-Path, full flow, review gates, confirmation, and Git authorization remain behaviorally intact. | Positive semantic assertions and Engineering/Spec review. |
| Historical closed cases are not modified and their state files remain readable as legacy evidence. | Changed-file scope and legacy compatibility wording. |
| Current case removes its initial state file and carries pre-closure status in `execution.md`. | Case artifact inspection before review. |
| Final closure is completed through `closure.md` status and the declared closure commit. | `doc-sync-close` Done Gate and final Git evidence; pending until closure. |
| Resumable closure statuses can deterministically return to execution without a second state file. | Shared resume truth table, context contract, handoff template, and derived state diagram. |
| Git authority no longer requires a separate closed-state item. | Authority/implementation consistency review. |
| Handoff contains no duplicate current status fields. | Template inspection and stale-field assertion. |
| No new skill, artifact type, phase, dependency, or runtime component is introduced. | Tree/diff comparison. |
| Modified Skills, YAML/frontmatter, links, and symlinks remain valid. | Validator, parser, link/symlink checks, `git diff --check`. |

## Tasks

### Task 1: Replace the shared state model

**Files:** shared protocol, loop protocol, router, retrieval routing, and plan
owner/template.

- [x] Capture the current state/cache/phase/routing-field contract.
- [x] Define artifact-owned statuses and deterministic precedence once.
- [x] Remove new-case state creation and the case-state template.
- [x] Preserve case identity, confirmation, artifact policy, and legacy reads.

### Task 2: Align execution, closure, review, and documentation

**Files:** execute/close/review owners and templates, workflow authority,
SSOT map, shareable workflow explanation, and case dashboard.

- [x] Add stage-owned frontmatter to execution and closure artifacts.
- [x] Replace state writes/repairs with artifact status transitions.
- [x] Remove duplicate routing-state claims from active documentation.
- [x] Delete this case's bootstrap state file after the new contract is valid.

### Task 3: Validate and review the complete delta

- [x] Run structural, semantic, stale-rule, link, symlink, and Git checks.
- [x] Record execution evidence and every acceptance criterion.
- [x] Run isolated Engineering and Spec Post-execution review.
- [ ] Fix/re-review material findings, then close only on aggregate `pass`.

### Task 4: Resolve independent review findings

- [ ] Define timestamp-based reopen semantics for resumable closure statuses.
- [ ] Align context resume rules, handoff shape, and derived state diagram.
- [ ] Replace the stale separate `closed state` requirement in Git authority.
- [ ] Keep closure-dependent acceptance pending until the closure commit.
- [ ] Refresh human summaries/dashboard, validate, commit one coherent fix, and
  rerun both independent review axes.

## Plan Amendments

| When | Change | Reason | User Confirmation | Impact |
|---|---|---|---|---|
| 2026-07-12T02:26:04+08:00 | v2 adds deterministic reopen semantics, handoff cleanup, Git authority alignment, and closure-owned final acceptance. | Independent Engineering and Spec review returned `changes_required`. | Approved 2026-07-12T12:41:15+08:00 | Adds three files and one approved-plan-amendment commit before the review fix. |

## Confirmation Log

| When | Confirmed By | Plan Version | Confirmation |
|---|---|---|---|
| 2026-07-12T02:09:03+08:00 | user | 1 | Approved current plan. |
| 2026-07-12T12:41:15+08:00 | user | 2 | Continued with the current amended plan. |
