---
case_id: 20260713-skill-dependency-boundaries
plan_version: 2
status: approved
risk_level: medium
approved_by: user
approved_at: 2026-07-13T17:14:52+0800
base_commit: 17f783f20cb4a168a2f07ece341b2df272384a7d
---

# Implementation Plan

## Human Approval Summary

- Outcome: make Skill composition explicit and stable: invoke other Skills by
  frontmatter name, keep multi-owner contracts under `_shared`, and treat a
  Skill's local `references/` and `templates/` as private by default.
- Material scope: normalize the existing review, TDD-exception, document-update,
  shared-protocol, and handoff dependency edges; update the architecture and
  distribution contracts without changing workflow behavior.
- Decision needed: confirm plan v2 before any Skill, shared reference, symlink,
  authority, or distribution file is changed.
- Expected local Git actions / commit count: create/switch to
  `codex/skill-dependency-boundaries`; 3 planned local commits (`plan`, one
  coherent refactor/docs unit, `closure`) plus one review-fix commit only if the
  mandatory review finds a material issue.
- Likely interruptions: stop if an audited cross-Skill rule has no unambiguous
  semantic owner, if the supported `skillshare` merge/copy distribution cannot
  preserve the proposed local reference paths, or if preserving review/TDD/
  authority behavior requires a material design change.
- Excluded actions: no new Skill, stage, artifact type, runtime, permanent
  dependency checker, invocation-name change, Constitution amendment, push,
  PR, merge, release, history rewrite, or unrelated case cleanup.

## Goal

Establish and apply one dependency rule for the active Skill tree:

1. Control-flow composition uses stable Skill names, never another Skill's
   filesystem path.
2. A contract used by multiple semantic owners lives in `_shared` and is
   exposed to each consumer through a local readable path.
3. Local `references/` and `templates/` belong to their Skill unless explicitly
   declared as a shared contract.
4. Control-flow cycles are allowed where the workflow requires them; physical
   cross-Skill import cycles and unused dependency edges are not.
5. `handoff.md` has split ownership: `_shared` owns its schema/template, the
   stage creating a real resume point owns that instance's content, and resume
   routing/context stages consume it without treating it as case status.

## Non-goals

- Do not make every Skill independently installable outside the supported
  repository-wide `skillshare` installation; required companion Skills may
  still be declared by stable invocation name.
- Do not change Post-execution review eligibility, review modes/verdicts,
  ad-hoc review authorization, TDD exception criteria, document lifecycle
  values, closure gates, or authority-patch permissions.
- Do not split the compact shared protocol or create a general dependency
  framework, registry, loader, or generated manifest.
- Do not rewrite unrelated Skill prose merely for stylistic consistency.

## Context

- Summary / brief / valid skip: workflow/Skill refactor. Current authority says
  each material rule has one semantic owner, cross-Skill invariants belong in
  `_shared`, and consumers retain only their local trigger/input/action/result/
  gate. The current tree mixes stable Skill-name routing, shared symlinks,
  direct imports of another Skill's files, implicit references without a path,
  and unused symlinks.
- Route verdict: `proceed_to_plan`. The Constitution, architecture, workflow,
  distribution authority, current Skill implementation, and supported install
  instructions provide sufficient consistent context. No active case or
  blocking authority conflict exists.
- Included sources:
  - `docs/authority/constitution.md`: minimum-path and human-semantic constraints;
    active owner-confirmed authority.
  - `docs/authority/architecture/repo-and-skills.md`: current loading and
    single-owner model; active code-backed authority.
  - `docs/authority/workflow/development-flow.md`: stage ownership and
    `tdd-execute` -> `review` behavior; active code-backed authority.
  - `docs/authority/operations/distribution.md` and `INSTALL.md`: supported
    `skillshare` discovery plus merge/copy symlink behavior.
  - Current `skills/**`: direct implementation evidence for every dependency
    edge.
  - `docs/cases/20260712-skill-context-cost-compression/`: historical evidence
    explaining why conditional loading and context-cost ceilings exist; it does
    not override current authority or Skill implementation.
- Excluded sources: archived designs, unrelated closed cases, generated product
  state, and repository-wide historical alternatives not needed to resolve the
  current dependency boundary.

## Workspace Baseline

- Base commit: `17f783f20cb4a168a2f07ece341b2df272384a7d`.
- Branch / run mode: current branch `main`; planned run mode `branch` on
  `codex/skill-dependency-boundaries` after confirmation.
- Dirty or existing changes: clean before this draft; this uncommitted plan is
  the only case-scoped change created during planning.

## Decisions

1. Replace `tdd-execute`'s direct read of
   `../../assessment/review/SKILL.md` with invocation of the installed `review`
   Skill in `Post-execution` mode. Keep the caller-owned input/result/fix-loop
   contract in `tdd-execute`; keep review procedure in `review`.
2. Move the existing TDD-exception contract to
   `skills/_shared/references/tdd-exceptions.md`; expose it as
   `references/tdd-exceptions.md` to both `plan-confirm` and `tdd-execute`.
3. Move the existing document-update/lifecycle contract to
   `skills/_shared/references/doc-update-rules.md`; expose it as
   `references/doc-update-rules.md` to both `setup-doc-governance` and
   `doc-sync-close`.
4. Clarify the shared artifact contract for `handoff.md`: `_shared` owns the
   template/field contract; `tdd-execute` or `doc-sync-close` owns a concrete
   file when it creates a real future resume point; `docloom-workflow` and
   `context-authority` may consume it for routing/revalidation; it never owns or
   overrides case status.
5. Make retained shared-protocol and handoff reads explicit and conditional in
   their owning `SKILL.md` files. Keep handoff-template exposure only for the
   current direct writers, `tdd-execute` and `doc-sync-close`. Remove the
   provably unused `review/references/shared-protocol.md` and
   `plan-confirm/templates/handoff.md` symlinks.
6. Document the dependency taxonomy in the existing architecture/Skill layout
   and distribution surfaces; add no new authority area or standalone policy
   artifact.

## Risk Level

Medium. The change is reversible Markdown/symlink refactoring with no public
Skill-name or workflow-outcome change, but it touches active workflow loading
and distributed Skill packaging. A broken edge could suppress a required
review, make TDD exception planning inconsistent, or leave copy-mode installs
with unreadable references. Exact behavior locks, symlink/path checks,
`skillshare` audit, context-cost comparison, and mandatory Engineering/Spec
review control the risk.

## TDD Applicability

- TDD Required: No.
- If No, reason and alternative verification: this is a Skill/document/
  symlink refactor without an executable workflow interpreter. Use
  `characterize -> refactor -> verify`: capture the current dependency graph
  and behavior assertions first; then validate exact path resolution, symlink
  integrity, supported distribution behavior, frontmatter/Markdown structure,
  context cost, Git delta, and fresh-agent workflow scenarios.

## Files to Change

Dependency consumers:

- `skills/development/tdd-execute/SKILL.md`
- `skills/development/plan-confirm/SKILL.md`
- `skills/development/doc-sync-close/SKILL.md`
- `skills/governance/setup-doc-governance/SKILL.md`

Shared-contract relocation and local exposure:

- Clarify handoff schema/instance/consumer ownership in
  `skills/_shared/references/shared-protocol.md` without changing artifact
  status semantics.
- Move `skills/development/tdd-execute/references/tdd-exceptions.md` to
  `skills/_shared/references/tdd-exceptions.md`.
- Add local symlinks at
  `skills/development/tdd-execute/references/tdd-exceptions.md` and
  `skills/development/plan-confirm/references/tdd-exceptions.md`.
- Move `skills/development/doc-sync-close/references/doc-update-rules.md` to
  `skills/_shared/references/doc-update-rules.md`.
- Add local symlinks at
  `skills/development/doc-sync-close/references/doc-update-rules.md` and
  `skills/governance/setup-doc-governance/references/doc-update-rules.md`.
- Remove `skills/assessment/review/references/shared-protocol.md`.
- Remove `skills/development/plan-confirm/templates/handoff.md`.
- Retain and explicitly bind
  `skills/development/tdd-execute/templates/handoff.md` and
  `skills/development/doc-sync-close/templates/handoff.md`.

Authority, maintainer, and distribution synchronization:

- `docs/authority/architecture/repo-and-skills.md`
- `docs/authority/operations/distribution.md`
- `skills/README.md`
- `INSTALL.md`

Workflow evidence and derived status:

- `docs/cases/20260713-skill-dependency-boundaries/plan.md`
- `docs/cases/20260713-skill-dependency-boundaries/execution.md`
- `docs/cases/20260713-skill-dependency-boundaries/closure.md`
- `docs/cases/README.md` when execution/closure status is synchronized.

## Acceptance Criteria

| Criterion | Planned verification |
|---|---|
| Active Skills contain no direct `../../<group>/<other-skill>/...` file dependency. | Exact `rg` search across `skills/**` returns none outside case/history evidence. |
| Cross-Skill control flow uses canonical frontmatter names and preserves all current authorization gates. | Inspect router, `tdd-execute`, `review`, shared ownership, and fresh-agent scenarios for explicit/ad-hoc/Post-execution entry. |
| TDD exceptions and document-update rules each have one shared source and readable local paths for every consumer. | Compare moved content byte-for-byte or semantically, inspect symlink targets, and read each path from the consumer directory. |
| Every retained local shared symlink has an explicit consumer; the two declared unused symlinks are gone. | Generate before/after symlink-to-consumer inventory and manually verify ambiguous semantic references. |
| `handoff.md` ownership is unambiguous: shared schema, producer-owned instance, resume-only consumption, and no status authority. | Inspect shared Artifact/Resume rules, confirm template symlinks exist only for direct writers, and run execution-pause plus resumable-closure scenarios. |
| No physical cross-Skill import cycle exists; workflow return routes remain allowed and behaviorally unchanged. | Build a small file-edge and Skill-route inventory; assert acyclic file edges and review the intentional route cycles separately. |
| Supported `skillshare` discovery and merge/copy behavior remain valid. | Run `skillshare audit ./skills --format json`, canonical Skill discovery checks, and non-mutating path/symlink validation consistent with `INSTALL.md`. |
| Default doorway, planning path, and full persistent-flow unique word costs do not exceed current ceilings or grow without a named reason. | Re-run the existing unique-path `wc -w` accounting and compare with `skills/README.md` ceilings. |
| Architecture and distribution authority accurately describe invocation, shared contracts, local privacy, and supported packaging. | Direct Spec comparison against the approved Decisions and final Skill tree. |
| Complete delta passes Engineering and Spec Post-execution review. | Review exact baseline-to-HEAD plus staged/unstaged/untracked case scope; resolve all Critical/Important findings and evidence gaps. |

## Tasks

### Task 1: Characterize the dependency graph and behavior locks

**Files:** current `skills/**`, authority/distribution sources, and case
execution evidence.

- [ ] Inventory canonical Skill-name routes, cross-Skill file paths, shared
  symlinks, implicit reads, and unused local assets.
- [ ] Record behavior locks for review authorization, Post-execution entry,
  TDD exception planning/execution, document lifecycle/update rules, handoff
  creation, and supported install modes.
- [ ] Separate handoff schema ownership, concrete-file producer ownership, and
  resume consumption from artifact status ownership.
- [ ] Confirm that only file dependencies—not intentional workflow return
  routes—must be acyclic.

### Task 2: Normalize invocation and shared contracts

**Files:** four consumer `SKILL.md` files, two shared references, local
reference/template symlinks.

- [ ] Replace the review filesystem import with canonical Skill-name
  invocation and preserve the exact caller/callee ownership boundary.
- [ ] Relocate TDD-exception and document-update contracts to `_shared` without
  changing their semantics; expose local consumer paths.
- [ ] Make retained shared/handoff reads explicit and conditional; remove only
  the two declared unused symlinks.
- [ ] Confirm that only `tdd-execute` and `doc-sync-close` expose the shared
  handoff template, while resume readers consume the case file directly.
- [ ] Re-run path, symlink, content-equivalence, and behavior-lock checks before
  proceeding.

### Task 3: Synchronize architecture and distribution contracts

**Files:** architecture/distribution authority, `skills/README.md`, `INSTALL.md`.

- [ ] Add the dependency taxonomy and local-reference privacy rule to the
  existing loading model.
- [ ] Document runtime companion-Skill invocation separately from physical
  shared-reference packaging.
- [ ] Update merge/copy symlink documentation for the two promoted shared
  contracts and remove stale statements.
- [ ] Verify word-cost ceilings and that no new ceremony, stage, manifest, or
  checker was introduced.

### Task 4: Verify, review, and close

**Files:** complete approved delta and case evidence.

- [ ] Run Markdown/frontmatter, link/path, symlink, `skillshare` audit,
  dependency-graph, word-cost, and Git checks.
- [ ] Run fresh-agent scenarios covering review invocation, missing/available
  TDD exception rules, governance lifecycle rules, handoff conditional loading,
  and standalone review.
- [ ] Invoke separate Engineering and Spec Post-execution axes, fix material
  findings atomically, and rerun affected axes.
- [ ] Map acceptance, write closure, refresh the case dashboard, create the
  declared closure commit, and report Done only after clean case scope.

## Post-Execution Review

- Engineering: verify path/symlink correctness, canonical Skill discovery,
  merge/copy readability, behavior-preserving contract moves, authorization
  gates, context-cost impact, and exact Git isolation.
- Spec: compare the complete delta with Goal, Non-goals, Decisions, Acceptance,
  Constitution, architecture/workflow/distribution authority, and the current
  supported install contract.
- Any unresolved Critical/Important finding returns to one smallest coherent
  `review-fix:<id>` commit and affected-axis re-review. Missing material
  evidence returns `insufficient_evidence`, never `pass`.

## Atomic Commit Strategy

| Unit | Planned title | Trailer step |
|---|---|---|
| Approved plan | `docs: approve skill dependency boundary plan` | `Doc-Loom-Step: plan` |
| Coherent implementation + authority/distribution sync | `refactor: normalize skill dependency boundaries` | `Doc-Loom-Step: task:dependency-boundaries` |
| Material review fix, only if needed | `fix: restore skill dependency contract` | `Doc-Loom-Step: review-fix:<id>` |
| Closure | `docs: close skill dependency boundary case` | `Doc-Loom-Step: closure` |

Every commit stages explicit case paths, passes relevant checks, uses
`Doc-Loom-Case: 20260713-skill-dependency-boundaries`, and excludes unrelated
work. No push, PR, merge, amend, rebase, squash, or history rewrite is
authorized by plan approval.

## Plan Amendments

| When | Change | Reason | User Confirmation | Impact |
|---|---|---|---|---|
| 2026-07-13 | Plan v2 clarifies `handoff.md` schema, instance, consumer, and status ownership; adds the shared protocol to the implementation scope. | The user explicitly asked who currently owns `handoff.md`. | Current-turn instruction approves revising the draft, not executing it. | No intended workflow behavior change; one existing shared file is added to the exact scope. |

## Confirmation Log

| When | Confirmed By | Plan Version | Confirmation |
|---|---|---:|---|
| 2026-07-13T17:14:52+0800 | user | 2 | User said `执行`, approving plan v2 and same-turn execution within its declared scope. |
