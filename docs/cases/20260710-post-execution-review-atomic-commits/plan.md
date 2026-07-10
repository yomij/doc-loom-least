---
case_id: 20260710-post-execution-review-atomic-commits
plan_version: 1
status: approved
risk_level: high
approved_by: user
approved_at: 2026-07-10T16:28:08+08:00
base_commit: c6dfe4612c341d26ba9d125d493781a4c527025f
---

# Implementation Plan

## Goal

Implement the approved requirements for mandatory post-execution review and
semantic atomic commits across the Doc Loom development workflow.

The completed workflow must:

- run a read-only Engineering and Spec review after eligible execution and
  before closure;
- return material findings to execution for verified atomic fix commits and
  re-review;
- create traceable atomic commits for approved requirements, approved plan,
  green tasks, verified refactors, review fixes, and closure;
- report unqualified `Done` only after the final review passes and the closure
  commit succeeds;
- preserve the constitutional minimum path by keeping Fast-Path compact and by
  adding no daemon, backend, persistent review phase, or mandatory review
  artifact.

## Non-goals

- Do not change the constitution.
- Do not add a CLI backend, daemon, scheduler, queue, remote service, GitHub
  Action, or centralized orchestrator.
- Do not make `grill` automatic or part of the development workflow.
- Do not require post-execution review for explanation-only, status-only, or
  read-only conversations.
- Do not require a `review.md` case artifact or a new case phase.
- Do not commit TDD Red states, failed attempts, timestamp-only changes, or one
  commit per checkbox.
- Do not push, merge, tag, release, amend, rebase, or squash commits.
- Do not regenerate the binary GIF/PNG workflow diagrams in this case. Update
  the textual and Mermaid workflow descriptions; record visual refresh as a
  follow-up only if the final review finds material user-facing drift.
- Do not modify archived historical designs as current facts.

## Context

- Summary / Brief: approved
  `docs/cases/20260710-post-execution-review-atomic-commits/requirements.md`.
- Route Verdict: `proceed_to_plan` after owner approval of requirements v1.
- Skipped Context Reason: none.

Context summary:

- Current authority and Skills make `review` manual-only, treat missing review
  as non-blocking, and disable staging/committing by default.
- The owner approved requirements v1 to replace those rules for eligible
  persistent cases with mandatory completed-change review and semantic atomic
  commits.
- The approved design uses separate Engineering and Spec axes, keeps review
  read-only, lets `tdd-execute` own the review/fix loop, and lets
  `doc-sync-close` own the final closure commit gate.
- The user-provided external code-review Skill contributed the two-axis,
  fixed-baseline, fail-fast, and isolated-context ideas. Its branch-only
  three-dot diff, hard issue-tracker dependency, and mandatory parallel-agent
  assumptions are not adopted.
- The constitution permits new gates only when they solve concrete failures
  through the smallest path. This plan therefore adds shared references and a
  semantic verifier, not a new runtime service or persistent workflow phase.

Context sources:

| Source | Layer | Why Included | Trust |
|---|---|---|---|
| `docs/authority/constitution.md` | Constitutional authority | Minimum path, human-first, and no-heavy-pipeline constraints | Highest |
| `docs/authority/workflow/development-flow.md` | Active authority | Current manual review and execution/closure semantics | High |
| `docs/authority/agent/policy.md` | Active authority | Agent authorization and high-risk workflow-policy boundary | High |
| `docs/authority/operations/git.md` | Active authority | Current commit-title contract | High |
| `skills/_shared/references/shared-protocol.md` | Current implementation | Shared routing, artifact, closure, Git, and review rules | High |
| `skills/development/plan-confirm/SKILL.md` | Current implementation | Plan approval, base commit, and authorization contract | High |
| `skills/development/tdd-execute/SKILL.md` | Current implementation | Execution, evidence, review risk, and optional commit behavior | High |
| `skills/assessment/review/SKILL.md` | Current implementation | Review evidence, maturity, severity, and read-only output | High |
| `skills/development/doc-sync-close/SKILL.md` | Current implementation | Acceptance, authority sync, and closure behavior | High |
| Approved requirements v1 | Confirmed case fact | Target behavior and acceptance object | High |

## Decisions

1. `tdd-execute` owns the mandatory post-execution review loop for eligible
   cases; `review` remains the read-only reviewer and does not write state.
2. Ad-hoc review remains available on explicit user request. Workflow-owned
   completed review is additionally authorized by an approved eligible plan.
3. Engineering and Spec are separate review axes. They may run sequentially in
   isolated passes; parallel sub-agents are optional and require runtime/user
   authorization.
4. `review_risk` remains a risk-depth signal. It is no longer the authorization
   source because every eligible case is reviewed; high risk strengthens the
   evidence and independence expected from the review.
5. No new case phase is added. The case remains `executing` until the aggregate
   review passes, then `tdd-execute` moves it to `doc_syncing`.
6. No mandatory `review.md` is added. Durable findings, fix commits, and final
   verdicts live in `execution.md`; closure consumes the final result.
7. Detailed review and commit rules are single-sourced in two disclosed shared
   references rather than duplicated across stage Skills.
8. Plan approval authorizes branch creation plus case-scoped staging and atomic
   commits described by this plan. It does not authorize push or history
   rewriting.
9. Commit traceability uses the existing `<type>: <summary>` title plus
   `Doc-Loom-Case` and `Doc-Loom-Step` trailers.
10. Existing cases remain compatible. Commit-aware closure derivation applies
    when the approved plan declares atomic commits required; legacy closed
    cases continue using their existing evidence.
11. A shell semantic verifier provides TDD-style Red/Green evidence for the
    Markdown and Skill contracts.
12. This case has one bootstrap exception: the approved requirements and draft
    plan were created under the old no-commit policy. After plan v1 approval and
    before behavior changes, execution will commit the approved requirements,
    then commit the approved plan and routing state as two separate atomic
    commits.
13. `docs/authority/agent/policy.md` will change `source_of_truth` from `adr` to
    `user_confirmed` because the new durable commit responsibility comes from
    this owner-confirmed case rather than an existing ADR. ADR-0002 remains a
    cited source for the unchanged human-first policy.

## Assumptions

- Git remains available during execution.
- Branch creation after plan approval is allowed by the approved run mode.
- `rg`, `bash`, Ruby, and standard Git commands remain available; no dependency
  installation is required.
- Current untracked files belong only to this case.
- The shared-protocol symlinks remain valid and do not require duplicate edits.
- Updating existing authority docs under the exact changes listed below is a
  confirmed narrow authority patch when plan v1 is explicitly approved.

## Workspace Baseline

- Proposed Base Commit:
  `c6dfe4612c341d26ba9d125d493781a4c527025f`
- Branch at plan creation: `main`
- Run Mode: `branch`
- Planned branch: `case/20260710-post-execution-review-atomic-commits`
- Dirty Workspace: yes; only the current untracked case directory.
- Existing Changed Files:
  - `docs/cases/20260710-post-execution-review-atomic-commits/requirements.md`
  - `docs/cases/20260710-post-execution-review-atomic-commits/case_state.yaml`
  - `docs/cases/20260710-post-execution-review-atomic-commits/plan.md`

## Risk Level

High.

Reason: the change modifies workflow and agent policy, authority facts, review
authorization, Git authorization, case-state derivation, and closure semantics.
These are High-Risk Topics under the shared protocol.

Plan confirmation policy: explicit confirmation of current plan v1 is required.
Plan approval authorizes same-turn execution, branch creation, and the scoped
atomic commits below unless the owner says plan-only, hold, revise, or review
first.

## TDD Applicability

- TDD Required: Yes.
- Strategy: semantic contract test.
- Red:
  - create `tools/verify-workflow-contracts.sh` with positive assertions for
    mandatory post-execution review and atomic commits plus negative assertions
    for stale manual-only/no-commit rules;
  - run it against current active authority, Skills, templates, and derived
    entry docs;
  - verify it fails for the expected stale-contract reasons.
- Green:
  - implement the approved workflow contracts;
  - run the same verifier until all required anchors pass and stale active rules
    are absent.
- Refactor:
  - consolidate repeated detail into the two shared reference files;
  - rerun the verifier, shell syntax check, link/symlink checks, and diff checks.

The Red script is not committed in a failing state. The observed failure is
recorded in `execution.md`; the script is committed together with the coherent
Green workflow change.

## Post-Execution Review Strategy

- Maturity: `Completed`.
- Engineering axis:
  - inspect the full case delta against the exact base commit;
  - check rule consistency, routing and state ownership, shell verifier quality,
    Git safety, compatibility, evidence integrity, and unnecessary complexity.
- Spec axis:
  - map the final delta to every requirement and acceptance criterion in
    requirements v1 and plan v1;
  - identify missing, partial, incorrect, or unrequested behavior.
- Context isolation:
  - run the two axes as separate sequential passes in this environment;
  - do not use one axis verdict as evidence for the other.
- Gate:
  - Critical or Important finding -> smallest fix, verification, atomic fix
    commit, and re-review;
  - insufficient evidence -> collect evidence and re-review;
  - only Minor or no findings -> pass to closure.
- Evidence:
  - record per-axis verdicts, findings, disposition, reviewed range, and commit
    mapping in `execution.md`.

## Atomic Commit Strategy

Every commit uses the current title standard plus trailers:

```text
Doc-Loom-Case: 20260710-post-execution-review-atomic-commits
Doc-Loom-Step: requirements
Doc-Loom-Step: plan
Doc-Loom-Step: task:core-workflow
Doc-Loom-Step: task:derived-docs
Doc-Loom-Step: review-fix:F1
Doc-Loom-Step: closure
```

Only one `Doc-Loom-Step` trailer is used in an individual commit. Review-fix
IDs increment from `F1` and are recorded in `execution.md` before staging.

Planned commits:

| Order | Semantic Step | Planned Title | Required Verification Before Commit |
|---:|---|---|---|
| 1 | `requirements` | `docs: approve post-execution review requirements` | Requirements status and YAML parse |
| 2 | `plan` | `docs: approve post-execution review plan` | Plan approval fields, case state, YAML parse |
| 3 | `task:core-workflow` | `feat: require post-execution review and atomic commits` | Expected Red observed earlier; verifier Green, `bash -n`, symlink check, diff check |
| 4 | `task:derived-docs` | `docs: document review and atomic commit workflow` | Verifier Green, stale wording grep, Markdown diff check |
| 5+ | Review fix `F1`, `F2`, and onward when needed | Exact `fix:` title is derived from the concrete root cause and recorded in `execution.md` before staging | Relevant regression check plus aggregate re-review |
| final | `closure` | `docs: close post-execution review and atomic commits case` | Final review pass, acceptance mapping, clean staged scope |

No commit may include unrelated user changes. Do not amend or squash these
commits under this plan.

## Adaptive Execution

Review fixes that stay within the approved goal, acceptance criteria, and file
responsibilities are allowed as minor deviations. Before editing, record in
`execution.md` the finding ID, exact files, exact `fix:` commit title, planned
verification, and expected impact.

Any fix that changes the goal, Non-goals, risk, TDD strategy, authority scope,
public contract, dependency/configuration scope, binary diagram scope, or file
responsibility is a material deviation and returns to `plan-confirm` for a new
plan version.

## Files to Change

Create:

- `skills/_shared/references/post-execution-review.md`
- `skills/_shared/references/atomic-commits.md`
- `tools/verify-workflow-contracts.sh`
- `docs/cases/20260710-post-execution-review-atomic-commits/execution.md`
- `docs/cases/20260710-post-execution-review-atomic-commits/closure.md`

Create distribution symlinks:

- `skills/development/plan-confirm/references/post-execution-review.md`
- `skills/development/plan-confirm/references/atomic-commits.md`
- `skills/development/tdd-execute/references/post-execution-review.md`
- `skills/development/tdd-execute/references/atomic-commits.md`
- `skills/assessment/review/references/post-execution-review.md`
- `skills/development/doc-sync-close/references/post-execution-review.md`
- `skills/development/doc-sync-close/references/atomic-commits.md`

Modify core authority and standards:

- `docs/authority/README.md`
- `docs/authority/architecture/repo-and-skills.md`
- `docs/authority/workflow/development-flow.md`
- `docs/authority/agent/policy.md`
- `docs/authority/operations/git.md`
- `docs/standards/git-commit-standard.md`

Modify shared and stage Skill contracts:

- `skills/_shared/references/shared-protocol.md`
- `skills/_shared/references/loop-protocol.md`
- `skills/development/docloom-workflow/SKILL.md`
- `skills/development/context-authority/SKILL.md`
- `skills/development/plan-confirm/SKILL.md`
- `skills/development/plan-confirm/templates/plan.md`
- `skills/development/tdd-execute/SKILL.md`
- `skills/development/tdd-execute/templates/execution.md`
- `skills/assessment/review/SKILL.md`
- `skills/assessment/review/references/complexity-only.md`
- `skills/development/doc-sync-close/SKILL.md`
- `skills/development/doc-sync-close/templates/closure.md`

Modify derived and operational documentation:

- `README.md`
- `README_CN.md`
- `CHANGELOG.md`
- `skills/README.md`
- `docs/index.md`
- `docs/ssot-map.md`
- `docs/product/current-state.md`
- `docs/share/workflow-and-design.md`
- `docs/cases/README.md`

Modify current case evidence:

- `docs/cases/20260710-post-execution-review-atomic-commits/requirements.md`
- `docs/cases/20260710-post-execution-review-atomic-commits/plan.md`
- `docs/cases/20260710-post-execution-review-atomic-commits/case_state.yaml`

The per-Skill `references/shared-protocol.md` paths are symlinks to the shared
source and must not be edited separately.

## File Responsibilities

| File / Group | Responsibility |
|---|---|
| `post-execution-review.md` | Eligibility, evidence resolution, Engineering/Spec axes, verdict aggregation, fix loop |
| `atomic-commits.md` | Authorization, semantic boundaries, commit health, trailers, mixed-worktree and closure rules |
| `shared-protocol.md` | Cross-skill vocabulary, ownership, state derivation, artifact and compatibility hooks |
| Stage `SKILL.md` files | Stage-specific invocation, sequencing, gates, and evidence ownership only |
| Authority docs | Confirmed durable workflow, agent, architecture, and Git policy facts |
| Templates | Minimal durable fields for planned review/commit strategy and actual evidence |
| `verify-workflow-contracts.sh` | Reproducible semantic guards against contract drift |
| Derived docs | Human-facing explanation consistent with authority and current Skills |
| Case docs | Approved intent, execution evidence, review findings, commits, closure |

## Acceptance Criteria

| Criteria | Planned Verification |
|---|---|
| Eligible persistent cases cannot reach successful closure without Completed post-execution review. | Semantic verifier plus inspection of shared protocol, `tdd-execute`, `review`, and `doc-sync-close`. |
| Engineering and Spec are separate axes with independent evidence and verdicts. | Inspect shared review reference and review output contract; run Spec mapping during final review. |
| Review uses exact plan baseline and includes committed, staged, unstaged, and untracked case changes. | Inspect review preflight rules and run final review against `c6dfe461...` plus `git status --short` and untracked listing. |
| Missing Spec or material evidence cannot pass. | Semantic verifier and review gate inspection. |
| Critical/Important findings return to execution, require verified atomic fix commits, and trigger re-review. | Inspect shared review reference, `tdd-execute` loop, and closure gates. |
| `review` remains read-only while supporting both explicit ad-hoc review and approved workflow-owned completed review. | Inspect review trigger, gates, shared ownership, and absence of review state writes. |
| No new case phase or mandatory `review.md` artifact is introduced. | Inspect shared phase/artifact tables and `rg` for new review artifact/phase declarations. |
| Approved requirements, approved plan, green tasks, review fixes, and closure have semantic atomic commit rules. | Inspect atomic commit reference, plan/tdd/closure Skills, templates, and Git authority. |
| Plan approval authorizes case-scoped stage/commit but not push or history rewriting. | Inspect plan confirmation, agent policy, shared Git authorization, and negative semantic guards. |
| Task/fix commits must be coherent, verified, revertible, and unrelated-change free. | Inspect atomic commit reference and `tdd-execute` commit gates. |
| Closure is not unqualified `Done` until review passes and closure commit succeeds. | Inspect commit-aware closure/state rules and `doc-sync-close` gates. |
| Legacy closed cases remain readable without retroactive commit requirements. | Inspect compatibility wording and review current closed case routing rules. |
| Fast-Path keeps compact review and commit outcomes without empty stages. | Inspect shared Fast-Path, plan/tdd/close rules, and derived usage docs. |
| Active stale rules such as review-always-manual and commit-disabled-by-default are removed or narrowed to ad-hoc/legacy scope. | Run semantic verifier and targeted `rg` over active authority, Skills, README, product, share, and standards docs. |
| Shared-protocol symlinks remain valid. | Run `find -L skills -type l -print`; expect no broken links. |
| Shell verifier is syntactically valid and passes. | Run `bash -n tools/verify-workflow-contracts.sh` and `bash tools/verify-workflow-contracts.sh`. |
| Markdown changes have no whitespace errors. | Run `git diff --check c6dfe4612c341d26ba9d125d493781a4c527025f HEAD` and worktree `git diff --check`. |
| Commit sequence, titles, trailers, and scopes match the approved strategy. | Inspect `git log --format='%H%n%B%n---' c6dfe4612c341d26ba9d125d493781a4c527025f..HEAD` and each commit diff. |
| Final workspace has no unexplained case-related changes before Done. | Run `git status --short`; expect clean after closure commit. |
| No CLI backend, daemon, scheduler, queue, automatic push, or unauthorized history rewrite is introduced. | Semantic verifier plus targeted source inspection. |

## Tasks

### Task 0: Bootstrap approved case commits

**Files:**

- Modify: `requirements.md`, `plan.md`, `case_state.yaml`

- [ ] Create branch
  `case/20260710-post-execution-review-atomic-commits` from
  `c6dfe4612c341d26ba9d125d493781a4c527025f` after plan approval.
- [ ] Validate requirements and case-state YAML with Ruby.
- [ ] Stage only the approved `requirements.md`; inspect the cached diff; commit
  `docs: approve post-execution review requirements` with case/step trailers.
- [ ] Stage the approved `plan.md` and planned `case_state.yaml`; inspect the
  cached diff; commit `docs: approve post-execution review plan` with trailers.
- [ ] Confirm the two commits contain no production/Skill behavior changes.

Expected result: the case has two clean, ordered bootstrap commits before any
workflow implementation change.

### Task 1: Establish a failing semantic contract

**Files:**

- Create: `tools/verify-workflow-contracts.sh`
- Create/update: `execution.md`

- [ ] Write positive assertions for mandatory completed review, two-axis
  semantics, atomic commit authorization/boundaries, and closure-commit gating.
- [ ] Write negative assertions for stale active rules including review-always-
  manual, no-auto-review, missing-review-nonblocking, and default-no-commit.
- [ ] Run `bash -n tools/verify-workflow-contracts.sh`; expect syntax pass.
- [ ] Run `bash tools/verify-workflow-contracts.sh`; expect Red caused by the
  current active contracts, not by a script defect.
- [ ] Record the exact expected Red evidence in `execution.md` without
  committing the failing script state.

Expected result: a reproducible failing contract proves the current workflow
does not satisfy requirements v1.

### Task 2: Implement core workflow, review, and Git contracts

**Files:**

- Create: `skills/_shared/references/post-execution-review.md`
- Create: `skills/_shared/references/atomic-commits.md`
- Modify all core authority, standards, shared protocol, stage Skill, reference,
  and template files listed under `Files to Change`.
- Update: `execution.md`, `plan.md` task progress.

- [ ] Add the two shared SSOT references and concise conditional pointers from
  the owning Skills.
- [ ] Create the approved per-Skill relative symlinks so skillshare copy-mode
  distribution carries the referenced contracts without duplicating content.
- [ ] Update shared ownership, route, artifact, state derivation, Fast-Path,
  degraded Git, and compatibility rules.
- [ ] Update `plan-confirm` to plan and authorize post-execution review and
  atomic commits.
- [ ] Update `tdd-execute` to commit green tasks, run the two-axis review, own
  the fix/re-review loop, and move to `doc_syncing` only after pass.
- [ ] Update `review` to support approved workflow-owned Completed review while
  remaining read-only and preserving explicit ad-hoc modes.
- [ ] Update `doc-sync-close` to consume final review/commit evidence and require
  a successful closure commit before unqualified Done.
- [ ] Apply the exact confirmed authority patches for workflow, agent, Git, and
  review-role facts; preserve constitution scope.
- [ ] Update templates with conditional review/commit strategy and evidence
  sections.
- [ ] Run the semantic verifier to Green, `bash -n`, symlink validation, and
  diff checks.
- [ ] Stage only the coherent core change and commit
  `feat: require post-execution review and atomic commits` with trailers.

Expected result: active authority and executable Skill contracts implement the
new behavior and the semantic verifier passes.

### Task 3: Synchronize derived and human-facing documentation

**Files:**

- Modify all files listed under `Modify derived and operational documentation`.
- Update: `execution.md`, `plan.md` task progress.

- [ ] Replace stale manual-only review and optional-commit guidance in English
  and Chinese entries.
- [ ] Update textual and Mermaid workflow descriptions to show execution,
  review/fix, closure, and commit boundaries.
- [ ] Update skill layout wording so `review` is both ad-hoc and workflow-owned,
  while `grill` remains manual.
- [ ] Update Git authority routes and SSOT labels from title-only wording to
  atomic commit policy wording.
- [ ] Update product current state and changelog with the confirmed behavior.
- [ ] Refresh the case dashboard to show this case waiting/executing as
  appropriate; defer final closed state to closure.
- [ ] Run the semantic verifier, stale wording checks, link/path inspection, and
  Markdown diff checks.
- [ ] Commit `docs: document review and atomic commit workflow` with trailers.

Expected result: current entry points and derived guidance agree with authority
and Skills without changing binary diagram assets.

### Task 4: Run mandatory post-execution review and fix loop

**Files:**

- Update: `execution.md`, `plan.md` task progress.
- Modify core or derived files only when a review finding requires correction.

- [ ] Confirm expected implementation commits exist and the case delta is
  explainable from the exact base commit.
- [ ] Run an isolated Engineering review pass.
- [ ] Run an isolated Spec review pass against requirements v1 and plan v1.
- [ ] Aggregate findings without allowing one axis to compensate for the other.
- [ ] For each material root cause, implement the smallest fix, verify it,
  create an atomic `fix:` commit with trailers, and re-run the affected and
  aggregate reviews.
- [ ] Record final per-axis verdicts, evidence gaps, findings disposition,
  reviewed range, and commit mapping in `execution.md`.

Expected result: aggregate review verdict is `pass`, or execution remains open
with explicit material findings/evidence gaps.

### Task 5: Verify, close, and create the closure commit

**Files:**

- Create: `closure.md`
- Modify: `case_state.yaml`, `plan.md`, `execution.md`, `docs/cases/README.md`
- Modify other derived docs only if final verified evidence requires a narrow
  synchronization already authorized by this plan.

- [ ] Run final semantic verifier, shell syntax, symlink, stale wording, diff,
  YAML, commit-log, and workspace-scope checks.
- [ ] Map every acceptance criterion to final evidence.
- [ ] Write closure and final review/commit summary before updating state.
- [ ] Update case state to closed only as part of the staged closure unit.
- [ ] Inspect the staged closure diff and create
  `docs: close post-execution review and atomic commits case` with trailers.
- [ ] Verify the closure commit contains the final case evidence and no new
  implementation fix.
- [ ] Run `git status --short`; do not report unqualified `Done` unless the
  closure commit succeeded and the case-related worktree is clean.

Expected result: the case is closed by a successful atomic closure commit and
all final evidence is durable.

## Planned Commands

Commands are run from repository root. Exact commit commands will use multiple
`-m` arguments so titles and trailers remain deterministic.

```text
git switch -c case/20260710-post-execution-review-atomic-commits
ruby -e 'require "yaml"; YAML.load_file("docs/cases/20260710-post-execution-review-atomic-commits/case_state.yaml"); %w[requirements.md plan.md].each { |name| path = "docs/cases/20260710-post-execution-review-atomic-commits/#{name}"; text = File.read(path); frontmatter = text.match(/\A---\n(.*?)\n---\n/m)&.[](1) or abort("missing frontmatter: #{path}"); YAML.safe_load(frontmatter, permitted_classes: [Time], aliases: false) }; puts "yaml: ok"'
git add docs/cases/20260710-post-execution-review-atomic-commits/requirements.md
git diff --cached --check
git diff --cached --stat
git commit -m 'docs: approve post-execution review requirements' -m 'Doc-Loom-Case: 20260710-post-execution-review-atomic-commits' -m 'Doc-Loom-Step: requirements'
git add docs/cases/20260710-post-execution-review-atomic-commits/plan.md docs/cases/20260710-post-execution-review-atomic-commits/case_state.yaml
git diff --cached --check
git diff --cached --stat
git commit -m 'docs: approve post-execution review plan' -m 'Doc-Loom-Case: 20260710-post-execution-review-atomic-commits' -m 'Doc-Loom-Step: plan'
bash -n tools/verify-workflow-contracts.sh
bash tools/verify-workflow-contracts.sh
ln -s ../../../_shared/references/post-execution-review.md skills/development/plan-confirm/references/post-execution-review.md
ln -s ../../../_shared/references/atomic-commits.md skills/development/plan-confirm/references/atomic-commits.md
ln -s ../../../_shared/references/post-execution-review.md skills/development/tdd-execute/references/post-execution-review.md
ln -s ../../../_shared/references/atomic-commits.md skills/development/tdd-execute/references/atomic-commits.md
ln -s ../../../_shared/references/post-execution-review.md skills/assessment/review/references/post-execution-review.md
ln -s ../../../_shared/references/post-execution-review.md skills/development/doc-sync-close/references/post-execution-review.md
ln -s ../../../_shared/references/atomic-commits.md skills/development/doc-sync-close/references/atomic-commits.md
find -L skills -type l -print
git add docs/authority/README.md docs/authority/architecture/repo-and-skills.md docs/authority/workflow/development-flow.md docs/authority/agent/policy.md docs/authority/operations/git.md docs/standards/git-commit-standard.md skills/_shared/references/shared-protocol.md skills/_shared/references/loop-protocol.md skills/_shared/references/post-execution-review.md skills/_shared/references/atomic-commits.md skills/development/docloom-workflow/SKILL.md skills/development/context-authority/SKILL.md skills/development/plan-confirm/SKILL.md skills/development/plan-confirm/references/post-execution-review.md skills/development/plan-confirm/references/atomic-commits.md skills/development/plan-confirm/templates/plan.md skills/development/tdd-execute/SKILL.md skills/development/tdd-execute/references/post-execution-review.md skills/development/tdd-execute/references/atomic-commits.md skills/development/tdd-execute/templates/execution.md skills/assessment/review/SKILL.md skills/assessment/review/references/post-execution-review.md skills/assessment/review/references/complexity-only.md skills/development/doc-sync-close/SKILL.md skills/development/doc-sync-close/references/post-execution-review.md skills/development/doc-sync-close/references/atomic-commits.md skills/development/doc-sync-close/templates/closure.md tools/verify-workflow-contracts.sh docs/cases/20260710-post-execution-review-atomic-commits/execution.md docs/cases/20260710-post-execution-review-atomic-commits/plan.md docs/cases/20260710-post-execution-review-atomic-commits/case_state.yaml
git diff --cached --check
git diff --cached --stat
git commit -m 'feat: require post-execution review and atomic commits' -m 'Doc-Loom-Case: 20260710-post-execution-review-atomic-commits' -m 'Doc-Loom-Step: task:core-workflow'
git add README.md README_CN.md CHANGELOG.md skills/README.md docs/index.md docs/ssot-map.md docs/product/current-state.md docs/share/workflow-and-design.md docs/cases/README.md docs/cases/20260710-post-execution-review-atomic-commits/execution.md docs/cases/20260710-post-execution-review-atomic-commits/plan.md
git diff --cached --check
git diff --cached --stat
git commit -m 'docs: document review and atomic commit workflow' -m 'Doc-Loom-Case: 20260710-post-execution-review-atomic-commits' -m 'Doc-Loom-Step: task:derived-docs'
git diff --check c6dfe4612c341d26ba9d125d493781a4c527025f HEAD
git diff --check
git status --short
git ls-files --others --exclude-standard
git log --format='%H%n%B%n---' c6dfe4612c341d26ba9d125d493781a4c527025f..HEAD
git add docs/cases/20260710-post-execution-review-atomic-commits/closure.md docs/cases/20260710-post-execution-review-atomic-commits/case_state.yaml docs/cases/20260710-post-execution-review-atomic-commits/execution.md docs/cases/20260710-post-execution-review-atomic-commits/plan.md docs/cases/README.md
git diff --cached --check
git diff --cached --stat
git commit -m 'docs: close post-execution review and atomic commits case' -m 'Doc-Loom-Case: 20260710-post-execution-review-atomic-commits' -m 'Doc-Loom-Step: closure'
git status --short
```

Staging commands will name explicit case-related paths after inspecting the
current diff. No wildcard staging and no `git add -A` are authorized.

## Risks

| Risk | Mitigation |
|---|---|
| The new review gate becomes ceremony | Fail on missing evidence, separate axes, compact Fast-Path, no new phase/artifact |
| Same-agent review repeats implementation bias | Isolated review passes; prefer independent reviewer when explicitly available |
| Two shared references increase protocol surface | Each has one clear SSOT responsibility and conditional pointers |
| Semantic grep tests become brittle | Test durable contract anchors and stale contradictions, not prose formatting |
| Commit policy creates noisy history | Commit semantic green units, not attempts or checkboxes |
| Mixed working tree captures unrelated files | Branch mode, explicit staging paths, cached diff inspection before every commit |
| Plan/requirements bootstrap violates future order | Record one transitional exception; commit them first after plan approval and before behavior changes |
| Closure file exists but commit fails | Commit-aware closure rule; no Done report until closure commit and clean scope |
| Legacy cases appear invalid | Apply commit-aware derivation only to plans declaring the new policy |
| Binary usage diagrams remain visually stale | Update textual/Mermaid flows now; final review decides whether a follow-up is material |
| Authority and Skills drift | One Green semantic verifier plus final two-axis review |

## Documentation Impact

Confirmed narrow authority patches on plan approval:

| Authority Doc | Confirmed Change Boundary |
|---|---|
| `docs/authority/workflow/development-flow.md` | Add mandatory eligible post-execution review, fix loop, commit gates, and commit-aware closure while preserving minimum path and legacy compatibility |
| `docs/authority/agent/policy.md` | Make approved case-scoped atomic commits an agent responsibility; reserve push/history rewrite and material deviations for explicit authorization; change `source_of_truth` from `adr` to `user_confirmed` while retaining ADR-0002 as a source |
| `docs/authority/operations/git.md` | Expand title-only authority into semantic atomic commit boundaries, health, scope, traceability, and closure rules |
| `docs/authority/architecture/repo-and-skills.md` | Describe `review` as both explicit ad-hoc assessment and workflow-owned read-only completed review; keep `grill` manual |
| `docs/authority/README.md` | Update authority routing labels for review role and Git commit policy |

Derived docs and operational inputs will be mechanically synchronized after the
core contracts are Green. The constitution remains unchanged.

## Confirmation Log

| When | Confirmed By | Plan Version | Confirmation |
|---|---|---:|---|
| 2026-07-10T16:28:08+08:00 | user | 1 | `批准。先提交，然后执行` |
