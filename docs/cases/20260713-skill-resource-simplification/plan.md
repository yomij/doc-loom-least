---
case_id: 20260713-skill-resource-simplification
plan_version: 1
status: approved
risk_level: medium
approved_by: user
approved_at: 2026-07-13T17:46:22+0800
base_commit: a353e47d366abdcc30cb59cd8d2e9bdc8f102db2
---

# Implementation Plan

## Human Approval Summary

- Outcome: simplify Doc Loom's Skill resource topology so references and
  templates exist only when they provide real progressive disclosure, shared
  ownership, or a stable artifact contract.
- Material scope: reduce canonical reference/template files from 18 toward 12,
  reduce shared symlinks from 13 toward 8, localize single-owner contracts,
  inline mandatory context, remove dead inspiration/input assets, merge the
  governance rulebook, and synchronize existing authority/distribution docs.
- Decision needed: confirm plan v1 before any Skill, reference, template,
  symlink, authority, or installation file is changed.
- Expected local Git actions / commit count: create/switch to
  `codex/skill-resource-simplification` from the exact baseline; 4 planned local
  commits (`plan`, resource topology, authority/distribution sync, `closure`)
  plus one review-fix commit only if required.
- Likely interruptions: stop if removing a resource changes a workflow outcome,
  if a retained template lacks a reliable producer/consumer contract, if
  merge/copy installation cannot preserve readable local files, or if exact
  resource-count reduction requires a material design change.
- Excluded actions: no new Skill, workflow stage, artifact type, runtime,
  permanent checker, Constitution amendment, execution/closure consolidation,
  push, PR, merge, release, history rewrite, or unrelated case cleanup.

## Goal

Apply the smallest coherent resource model supported by the current workflow:

1. Keep `_shared` only for contracts with multiple real semantic owners.
2. Keep external references only for a named branch or substantial rulebook;
   inline material that every invocation must load.
3. Keep templates only when a Skill has an exact conditional or mandatory
   producer pointer.
4. Preserve one semantic source of truth without using symlinks to compensate
   for weak ownership boundaries.
5. Preserve all current invocation, authorization, status, TDD, review,
   governance, closure, handoff, and distribution behavior.

## Non-goals

- Do not split `shared-protocol.md` into more topic references.
- Do not merge `plan.md`, `execution.md`, `closure.md`, or `handoff.md` templates.
- Do not change review modes, risk outcomes, TDD exception eligibility,
  document lifecycle values, artifact statuses, confirmation gates, or final
  closure requirements.
- Do not make every Skill independently installable outside the supported
  repository-wide `skillshare` installation.
- Do not rewrite unrelated Skill prose for style alone.

## Context

- Summary / brief / valid skip: the explicit complexity-only review found that
  the current tree has 11 canonical references, 7 canonical templates, and 13
  local symlinks. Several resources are always loaded, have only one consumer,
  duplicate an existing owner, have a weak context pointer, or have no producer
  pointer at all. The previous dependency-boundary case is closed and the
  worktree is clean.
- Route verdict: `proceed_to_plan`. Active constitution, architecture,
  workflow, distribution, current Skills, the completed dependency-boundary
  case, and direct resource/consumer inventory provide sufficient consistent
  evidence. No active case or blocking authority conflict exists.
- Included sources:
  - `docs/authority/constitution.md`: minimum-path and anti-ceremony constraints;
    active owner-confirmed authority.
  - `docs/authority/architecture/repo-and-skills.md`: progressive disclosure,
    shared-contract, local-privacy, and semantic-owner model; active code-backed
    authority.
  - `docs/authority/workflow/development-flow.md`: current stage and artifact
    ownership; active code-backed authority.
  - `docs/authority/operations/distribution.md` and `INSTALL.md`: supported
    repository-wide merge/copy packaging behavior.
  - Current `skills/**`: direct implementation and resource-consumer evidence.
  - `docs/cases/20260713-skill-dependency-boundaries/`: completed predecessor
    evidence for current symlink and dependency behavior.
- Excluded sources: archived designs, unrelated historical cases, generated
  product state, and speculative future lifecycle domains.

## Workspace Baseline

- Base commit: `a353e47d366abdcc30cb59cd8d2e9bdc8f102db2`.
- Branch / run mode: current branch `codex/skill-dependency-boundaries`; planned
  run mode `branch` on `codex/skill-resource-simplification` from the exact
  baseline after confirmation.
- Dirty or existing changes: clean before case creation; this draft plan and
  the derived case-dashboard row are the only new case-scoped changes.

## Decisions

1. Retain `skills/_shared/references/shared-protocol.md` as the single shared
   workflow kernel; do not split it.
2. Move the single-consumer loop protocol into
   `docloom-workflow/references/loop-protocol.md` as a real local file.
3. Move TDD exception criteria into
   `plan-confirm/references/tdd-exceptions.md`; execution consumes the approved
   plan exception and no longer loads the criteria reference.
4. Move full document-update rules into
   `doc-sync-close/references/doc-update-rules.md`; governance owns its required
   lifecycle enum locally rather than importing the full closure contract.
5. Merge `context-resolution.md` and `retrieval-routing.md` into a compressed
   `context-authority/SKILL.md`, because every context run currently loads both.
6. Remove `plan-confirm/references/risk-levels.md`; make the shared protocol the
   complete cross-skill risk classification source used by planning.
7. Remove `grill/references/challenge-prompts.md` and
   `docloom-workflow/templates/product-current-state.md`; the former is generic
   inspiration and the latter has no current producer pointer.
8. Merge governance layering/routing and verdict rules into one local
   `references/governance-rules.md`; rename the plan template to
   `templates/governance-plan.md` and bind it directly from the owning Skill.
9. Retain `complexity-only.md`, the context brief template, plan/execution/
   closure templates, and shared handoff template. Add exact template pointers
   where a retained producer is currently implicit.
10. Update only existing authority/distribution boundaries and derived install/
    layout guidance needed to describe the final tree.

## Risk Level

Medium. The change is reversible Markdown/symlink refactoring with no intended
public invocation or workflow-outcome change, but it touches active Skill
loading, semantic ownership, and distributed package contents. Incorrect
localization could suppress a required rule or leave installed Skills with
missing files. Exact behavior locks, consumer/path inventory, merge/copy
simulation, cost checks, and Engineering/Spec review control the risk.

## TDD Applicability

- TDD Required: No.
- If No, reason and alternative verification: this is a Skill/document/resource
  topology refactor without an executable workflow interpreter. Use the allowed
  behavior-preserving refactor exception: `characterize -> refactor -> verify`.
  Capture current resource counts, pointers, ownership, symlink graph, behavior
  anchors, and loading costs; then validate final paths, content, packaging,
  semantic assertions, and fresh invocation scenarios.

## Files to Change

Skill resources and owners:

- `skills/_shared/references/shared-protocol.md`
- `skills/_shared/references/loop-protocol.md`
- `skills/_shared/references/tdd-exceptions.md`
- `skills/_shared/references/doc-update-rules.md`
- `skills/development/docloom-workflow/references/loop-protocol.md`
- `skills/development/docloom-workflow/templates/product-current-state.md`
- `skills/development/context-authority/SKILL.md`
- `skills/development/context-authority/references/context-resolution.md`
- `skills/development/context-authority/references/retrieval-routing.md`
- `skills/development/plan-confirm/SKILL.md`
- `skills/development/plan-confirm/references/risk-levels.md`
- `skills/development/plan-confirm/references/tdd-exceptions.md`
- `skills/development/tdd-execute/SKILL.md`
- `skills/development/tdd-execute/references/tdd-exceptions.md`
- `skills/development/doc-sync-close/SKILL.md`
- `skills/development/doc-sync-close/references/doc-update-rules.md`
- `skills/assessment/grill/SKILL.md`
- `skills/assessment/grill/references/challenge-prompts.md`
- `skills/governance/setup-doc-governance/SKILL.md`
- `skills/governance/setup-doc-governance/references/layering-and-routing.md`
- `skills/governance/setup-doc-governance/references/verdicts.md`
- `skills/governance/setup-doc-governance/references/governance-rules.md`
- `skills/governance/setup-doc-governance/references/doc-update-rules.md`
- `skills/governance/setup-doc-governance/templates/GOVERNANCE_PLAN.md`
- `skills/governance/setup-doc-governance/templates/governance-plan.md`

Authority, distribution, and derived guidance:

- `docs/authority/architecture/repo-and-skills.md`
- `docs/authority/workflow/development-flow.md`
- `docs/authority/workflow/doc-governance.md`
- `docs/authority/agent/policy.md`
- `docs/authority/operations/distribution.md`
- `skills/README.md`
- `INSTALL.md`

Case evidence and dashboard:

- `docs/cases/20260713-skill-resource-simplification/plan.md`
- `docs/cases/20260713-skill-resource-simplification/execution.md`
- `docs/cases/20260713-skill-resource-simplification/closure.md`
- `docs/cases/README.md`

## Acceptance Criteria

| Criterion | Planned verification |
|---|---|
| Canonical resource files reduce from 18 to no more than 12 without behavior loss. | Count real files under `skills/**/references` and `skills/**/templates`, excluding symlinks; compare baseline/final inventory. |
| Shared symlinks reduce from 13 to no more than 8 and every retained symlink has multiple real owners/writers. | `find skills -type l`, resolve targets, and map each retained edge to explicit consumers. |
| `_shared` contains only the multi-owner shared protocol and handoff template. | Inspect final `_shared` tree and consumer graph. |
| Every retained external reference is branch-specific or a substantial co-located rulebook. | Inspect every Skill context pointer and final resource disposition table. |
| Every retained template has an exact producer pointer and stable artifact owner. | Search exact `templates/<name>` pointers and compare with artifact ownership. |
| Invocation, authorization, status, risk, TDD, review, handoff, governance, closure, and Git boundaries remain unchanged. | Baseline/final semantic anchor assertions plus focused fresh-agent scenarios. |
| Merge and copy installations contain readable required local files with no broken links. | `skillshare audit`, symlink/path validation, and non-mutating `cp -RL` simulation. |
| Default doorway, context-through-plan, typical full-flow, and active corpus costs remain within current ceilings and do not grow without a named reason. | Re-run unique-path `wc -w` accounting against `skills/README.md`. |
| Authority, README, and install guidance name the final resource model and contain no stale old paths. | Exact source-path searches and Spec comparison. |
| Complete delta passes Engineering and Spec Post-execution review. | Review exact baseline-to-final delta, commits, worktree, behavior locks, and acceptance evidence. |

## Tasks

### Task 1: Characterize resource loading and behavior locks

**Files:** current `skills/**`, relevant authority/distribution files, and case
execution evidence.

- [ ] Record canonical reference/template counts, symlink graph, exact context
  pointers, direct and indirect consumers, unique loading-path word costs, and
  unbound resources.
- [ ] Record behavior locks for case routing, risk, TDD exceptions, document
  lifecycle/update rules, governance verdicts, review/grill boundaries,
  handoff, closure, and supported install modes.
- [ ] Confirm the proposed final owner and retained/deleted disposition for
  every current canonical reference and template.

### Task 2: Reduce and localize Skill resources

**Files:** listed Skill/reference/template paths.

- [ ] Localize loop, TDD-exception, and document-update contracts to their
  actual semantic owners and remove obsolete shared symlinks.
- [ ] Merge mandatory context references into `context-authority/SKILL.md` and
  consolidate risk classification without duplicating semantics.
- [ ] Remove generic/dead assets and merge governance rules into one reference.
- [ ] Bind every retained template through an exact condition/path; preserve
  stage-owned and shared template contracts.
- [ ] Re-run path, pointer, count, behavior-anchor, and word-cost checks.

### Task 3: Synchronize authority and distribution surfaces

**Files:** listed authority docs, `skills/README.md`, and `INSTALL.md`.

- [ ] Update existing loading/ownership/distribution statements and source
  paths to match the final tree without adding a new authority area.
- [ ] Remove exhaustive lists or wording that would immediately become stale
  when a private local reference changes.
- [ ] Validate authority frontmatter, links, shared/copy installation behavior,
  and stale-path searches.

### Task 4: Verify, review, and close

**Files:** complete approved delta and case evidence.

- [ ] Run Skill/frontmatter, Markdown, symlink/path, `skillshare`, copy-mode,
  semantic behavior, word-cost, and exact Git checks.
- [ ] Run focused fresh-agent scenarios for the changed pointers and owners.
- [ ] Invoke separate Engineering and Spec Post-execution axes, fix material
  findings atomically, and rerun affected axes.
- [ ] Map acceptance, write closure, refresh the case dashboard, create the
  declared closure commit, and report Done only after clean case scope.

## Post-Execution Review

- Engineering: verify no missing conditional rule, broken local path/symlink,
  unreadable copy install, trigger regression, duplicated owner, unbound
  template, unexplained resource, cost regression, or mixed Git scope.
- Spec: compare the complete delta with Goal, Non-goals, Decisions, Acceptance,
  Constitution, architecture/workflow/distribution authority, and the current
  supported install contract.
- Any unresolved Critical/Important finding returns to one smallest coherent
  `review-fix:<id>` commit and affected-axis re-review. Missing material
  evidence returns `insufficient_evidence`, never `pass`.

## Atomic Commit Strategy

| Unit | Planned title | Trailer step |
|---|---|---|
| Approved plan | `docs: approve skill resource simplification plan` | `Doc-Loom-Step: plan` |
| Resource topology and Skill pointers | `refactor: simplify skill resources` | `Doc-Loom-Step: task:resource-topology` |
| Authority/distribution synchronization | `docs: sync simplified skill resource model` | `Doc-Loom-Step: task:resource-docs` |
| Material review fix, only if needed | `fix: restore skill resource contract` | `Doc-Loom-Step: review-fix:<id>` |
| Closure | `docs: close skill resource simplification case` | `Doc-Loom-Step: closure` |

Every commit stages explicit case paths, passes relevant checks, uses
`Doc-Loom-Case: 20260713-skill-resource-simplification`, and excludes unrelated
work. No push, PR, merge, amend, rebase, squash, or history rewrite is
authorized by plan approval.

## Confirmation Log

| When | Confirmed By | Plan Version | Confirmation |
|---|---|---:|---|
| 2026-07-13T17:46:22+0800 | user | 1 | User said `get things done`, approving plan v1 and same-turn execution within its declared scope. |
