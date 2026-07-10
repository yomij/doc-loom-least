---
case_id: 20260710-post-execution-review-atomic-commits
plan_version: 2
status: approved
risk_level: high
approved_by: user
approved_at: 2026-07-10T17:10:43+08:00
base_commit: 763025a586e26ecba6a5f1641ab7d4288ad2662d
---

# Implementation Plan

## Goal

Implement requirements v1 through the smallest workflow change:

- add a workflow-owned `Post-execution` mode directly to `review`;
- require `tdd-execute` to run it after eligible implementation and own the
  fix/re-review loop;
- require semantic atomic commits for the approved plan, green tasks, review
  fixes, and closure;
- let `doc-sync-close` report unqualified `Done` only after review passes and
  the closure commit succeeds.

## Non-goals

- Do not create shared post-execution-review or atomic-commit reference files.
- Do not create new reference symlinks or a workflow-verifier script.
- Do not create a new case phase or mandatory `review.md` artifact.
- Do not modify the constitution or general Agent Policy.
- Do not make `grill` automatic.
- Do not push, merge, tag, release, amend, rebase, or squash commits.
- Do not regenerate binary workflow diagrams.
- Do not modify archived historical designs.

## Context

- Requirements: approved
  `docs/cases/20260710-post-execution-review-atomic-commits/requirements.md`.
- Prior plan: v1 was approved and committed, but no workflow behavior commit
  was created.
- Revision reason: the owner challenged the two new shared references, seven
  distribution symlinks, 138-line verifier, and broad authority surface as
  over-engineering.
- User decision: keep the requirements, discard the uncommitted v1 execution
  attempt, and revise to the smallest design centered on `review/SKILL.md`.
- Route Verdict: `proceed_to_plan` with material plan revision.

Current facts:

- Requirements commit: `1cb58fd`.
- Approved plan v1 commit: `763025a`.
- All uncommitted v1 implementation changes were removed before this draft.
- Current workspace is clean before writing plan v2.
- Current branch is `case/20260710-post-execution-review-atomic-commits`.

## Decisions

1. Detailed Engineering/Spec review behavior lives only in
   `skills/assessment/review/SKILL.md`.
2. `tdd-execute` contains only stage-owned review timing, fix-loop, task commit,
   and review-fix commit rules.
3. `doc-sync-close` contains only final review evidence and closure commit
   gates.
4. `plan-confirm` records review/commit strategy and makes current-plan approval
   authorize case-scoped staging and commits.
5. `shared-protocol.md` receives only the minimum cross-skill exception to the
   existing manual-review rule plus shared closure/authorization semantics.
6. `docloom-workflow`, `context-authority`, and `loop-protocol` receive only
   narrow wording changes where their absolute manual-only rule would conflict.
7. Atomic commit detail is stage-owned; durable repository-level principles are
   summarized in `docs/authority/operations/git.md`.
8. No separate shared references, symlinks, or semantic verifier are created.
9. Verification uses targeted `rg`, diff, Git history, YAML, and a mandatory
   final Engineering/Spec review.
10. Plan v1 remains immutable history. Plan v2 supersedes it before any behavior
    implementation commit.

## Workspace Baseline

- Proposed Base Commit:
  `763025a586e26ecba6a5f1641ab7d4288ad2662d`
- Branch: `case/20260710-post-execution-review-atomic-commits`
- Run Mode: `branch`
- Dirty Workspace Before Plan v2: no.
- Current Change: draft plan v2 and case routing state only.

## Risk Level

High.

Reason: this still changes workflow policy, review authorization, Git
authorization, and closure semantics. The revision reduces implementation
surface but not the policy risk.

Plan v2 requires explicit confirmation. Approval authorizes same-turn execution
and the case-scoped commits below unless the owner says hold or plan-only.

## TDD Applicability

- TDD Required: No.
- Exception category: Pure documentation change.
- Reason: the executable product surface is Markdown Skill contracts and
  authority text; the repository has no workflow interpreter or runtime test
  suite. The discarded verifier would test wording anchors rather than runtime
  behavior.
- Alternative Verification:
  - targeted positive and stale-rule `rg` checks;
  - `git diff --check` before each commit;
  - YAML/frontmatter parsing for case artifacts;
  - broken-symlink check;
  - per-commit scope and trailer inspection;
  - mandatory final Engineering and Spec review through the updated `review`
    Skill.

## Post-Execution Review Strategy

- Mode: `Post-execution` in `review/SKILL.md`.
- Maturity: `Completed`.
- Engineering axis: correctness of workflow ownership, state transitions,
  commit safety, evidence integrity, compatibility, and unnecessary complexity.
- Spec axis: final diff against requirements v1 and plan v2, including missing,
  partial, incorrect, or unrequested behavior.
- Isolation: separate sequential passes; no sub-agent dependency.
- Gate:
  - Critical/Important -> smallest fix, verification, atomic fix commit,
    re-review;
  - insufficient evidence -> collect evidence and re-review;
  - only Minor/no findings -> pass to closure.
- Durable result: record final axis verdicts and material findings in
  `execution.md`; no `review.md`.

## Atomic Commit Strategy

Plan approval authorizes explicit staging for this case only.

| Order | Step | Commit Title |
|---:|---|---|
| 1 | Approved plan v2 | `docs: approve simplified review and commit plan` |
| 2 | Core workflow behavior | `feat: require post-execution review and atomic commits` |
| 3 | Directly conflicting derived docs | `docs: document post-execution review workflow` |
| 4+ | Review fixes, when needed | Exact `fix:` title recorded before staging |
| Final | Closure | `docs: close post-execution review and atomic commits case` |

Every commit uses:

```text
Doc-Loom-Case: 20260710-post-execution-review-atomic-commits
Doc-Loom-Step: plan-v2 | task:core-workflow | task:derived-docs | review-fix:F1 | closure
```

No broad staging, unrelated files, push, or history rewriting is authorized.

## Files to Change

Core behavior and authority:

- `skills/assessment/review/SKILL.md`
- `skills/assessment/review/references/complexity-only.md`
- `skills/development/tdd-execute/SKILL.md`
- `skills/development/tdd-execute/templates/execution.md`
- `skills/development/doc-sync-close/SKILL.md`
- `skills/development/doc-sync-close/templates/closure.md`
- `skills/development/plan-confirm/SKILL.md`
- `skills/development/plan-confirm/templates/plan.md`
- `skills/development/docloom-workflow/SKILL.md`
- `skills/development/context-authority/SKILL.md`
- `skills/_shared/references/shared-protocol.md`
- `skills/_shared/references/loop-protocol.md`
- `docs/authority/workflow/development-flow.md`
- `docs/authority/operations/git.md`
- `docs/authority/architecture/repo-and-skills.md`

Directly conflicting derived docs:

- `README.md`
- `README_CN.md`
- `skills/README.md`
- `docs/product/current-state.md`
- `docs/share/workflow-and-design.md`
- `CHANGELOG.md`

Case evidence:

- `docs/cases/20260710-post-execution-review-atomic-commits/plan.md`
- `docs/cases/20260710-post-execution-review-atomic-commits/case_state.yaml`
- create `docs/cases/20260710-post-execution-review-atomic-commits/execution.md`
- create `docs/cases/20260710-post-execution-review-atomic-commits/closure.md`
- `docs/cases/README.md` at closure

## File Responsibilities

| Owner | Responsibility |
|---|---|
| `review/SKILL.md` | Complete post-execution preflight, Engineering/Spec axes, aggregation, findings contract |
| `tdd-execute/SKILL.md` | Eligibility, invocation timing, fix/re-review loop, task and review-fix commits |
| `doc-sync-close/SKILL.md` | Consume passing verdict and create closure commit |
| `plan-confirm/SKILL.md` | Record strategy and authorize declared commits on approval |
| `shared-protocol.md` | Minimal cross-skill ownership, route, closure, and authorization rules |
| Workflow/Git authority | Durable policy summary, not detailed implementation manual |
| Templates | Conditional strategy and evidence headings only |
| Derived docs | Remove direct contradictions and explain the final user-visible flow |

## Acceptance Criteria

| Criteria | Planned Verification |
|---|---|
| `review` directly owns a `Post-execution` mode with separate Engineering and Spec axes. | Inspect `review/SKILL.md`; targeted positive `rg`. |
| Review covers the approved plan/spec, complete current diff, tests/evidence, scope creep, errors, contracts, and complexity. | Inspect mode preflight and axis checks. |
| Either axis with Critical/Important findings returns to execution; missing material evidence cannot pass. | Inspect review aggregate verdict and `tdd-execute` fix loop. |
| `review` remains read-only and ad-hoc review remains explicit. | Inspect review gates and shared wording. |
| Eligible execution invokes Post-execution review automatically under the approved plan. | Inspect `tdd-execute` workflow and narrowed manual-only rules. |
| Task and review-fix commits are atomic, verified, case-scoped, and unrelated-change free. | Inspect `tdd-execute` commit section and Git authority. |
| Plan approval authorizes declared case commits but not push or history rewriting. | Inspect `plan-confirm`, shared confirmation semantics, and Git authority. |
| Closure requires passing review evidence and a successful closure commit before unqualified `Done`. | Inspect `doc-sync-close`, closure template, shared closure rule. |
| No new reference, symlink, verifier, case phase, or `review.md` artifact is introduced. | Inspect final file list, `git status`, and shared artifact/phase tables. |
| Fast-Path uses compact review/commit behavior without empty stages. | Inspect shared, plan, execute, and close wording. |
| Legacy cases are not retroactively invalidated. | Inspect shared/closure compatibility wording. |
| Directly conflicting manual-only statements are removed or narrowed. | Run stale-rule `rg` across active Skills, authority, README, product, and share docs. |
| No broken Skill symlink exists. | Run `find -L skills -type l -print`; expect empty. |
| Markdown and staged changes are clean. | Run `git diff --check` and `git diff --cached --check`. |
| Commit sequence, titles, trailers, and scope match plan v2. | Inspect `git log` and per-commit diffs from `763025a`. |
| Final Engineering and Spec review both pass. | Record separate verdicts in `execution.md`. |

## Tasks

### Task 0: Commit approved plan v2

**Files:** `plan.md`, `case_state.yaml`

- [x] On approval, write plan v2 approval fields and Confirmation Log.
- [x] Set case state to `planned` with current plan version 2.
- [x] Parse YAML/frontmatter and inspect the staged diff.
- [x] Commit `docs: approve simplified review and commit plan` with
  `Doc-Loom-Step: plan-v2`.

Expected result: plan v2 supersedes v1 before behavior changes.

### Task 1: Implement the minimal core workflow

**Files:** all paths under `Core behavior and authority`, plus `plan.md`,
`case_state.yaml`, and `execution.md`.

- [x] Add the full `Post-execution` mode directly to `review/SKILL.md`.
- [x] Add only stage-owned invocation, fix loop, and commit rules to
  `tdd-execute` and `doc-sync-close`.
- [x] Add review/commit strategy and approval authorization to `plan-confirm`.
- [x] Narrow absolute manual-only wording in shared/router/context/loop rules.
- [x] Apply concise workflow, Git, and architecture authority patches.
- [x] Update conditional template headings.
- [x] Run core positive/stale-rule, YAML, symlink, and diff checks.
- [x] Commit `feat: require post-execution review and atomic commits` with
  `Doc-Loom-Step: task:core-workflow`.

Expected result: the workflow behavior is complete without new references,
symlinks, scripts, phases, or artifacts.

### Task 2: Synchronize directly conflicting derived docs

**Files:** all paths under `Directly conflicting derived docs`, plus `plan.md`
and `execution.md`.

- [x] Replace manual-only guidance with explicit ad-hoc plus workflow-owned
  post-execution review wording.
- [x] Update text/Mermaid flow and Git/commit explanation without touching
  binary diagrams.
- [x] Run stale-rule, path/link, and diff checks.
- [x] Commit `docs: document post-execution review workflow` with
  `Doc-Loom-Step: task:derived-docs`.

Expected result: active entry docs no longer contradict the core workflow.

### Task 3: Run final review and fix loop

**Files:** `execution.md`, `plan.md`, and only files required by concrete
findings.

- [x] Run the Engineering pass in `review` Post-execution mode.
- [x] Run the Spec pass separately against requirements v1 and plan v2.
- [x] Aggregate verdicts without cross-axis compensation.
- [x] For each material root cause, record exact files/title/verification,
  create an atomic `fix:` commit, and re-review.
- [x] Record final verdicts and findings disposition in `execution.md`.

Expected result: both axes and the aggregate verdict pass.

### Task 4: Close with an atomic closure commit

**Files:** create `closure.md`; update `case_state.yaml`, `plan.md`,
`execution.md`, and `docs/cases/README.md`.

- [x] Run final targeted checks, YAML parse, Git log, diff, and workspace scope.
- [x] Map all acceptance criteria to evidence.
- [x] Write closure and closed state without adding implementation fixes.
- [x] Commit `docs: close post-execution review and atomic commits case` with
  `Doc-Loom-Step: closure`.
- [x] Report unqualified `Done` only after commit success and clean status.

Expected result: final evidence is durable and the case-related worktree is
clean.

## Planned Commands

```text
ruby -e 'require "yaml"; YAML.load_file("docs/cases/20260710-post-execution-review-atomic-commits/case_state.yaml"); puts "yaml: ok"'
rg -n 'Post-execution|Engineering Axis|Spec Axis|Atomic Commits|closure commit' skills docs/authority
rg -n 'No automatic review triggers|不自动触发 Review，Review 始终是手动行为|Do not auto-trigger `review`, even when `review_risk` is high.|Do not trigger `review`; only the user can request it.|Default: do not stage or commit.|Missing review conversation is not by itself a closure blocker.' skills docs/authority README.md README_CN.md docs/product/current-state.md docs/share/workflow-and-design.md
find -L skills -type l -print
git diff --check
git diff --cached --check
git status --short
git log --format='%H%n%B%n---' 763025a586e26ecba6a5f1641ab7d4288ad2662d..HEAD
```

The stale-rule `rg` is expected to return no active contradictions after the
corresponding task. Explicit staging paths and exact commit commands are taken
from the approved Atomic Commit Strategy; broad staging is not authorized.

## Risks

| Risk | Mitigation |
|---|---|
| Review details leak into multiple Skills | Keep the complete mode only in `review`; other Skills consume its verdict |
| Atomic commit rules duplicate | Keep only stage-owned rules plus a concise Git authority summary |
| Manual-only wording remains elsewhere | Targeted stale-rule search over active sources |
| Same-agent review bias | Separate Engineering and Spec passes; no shared verdict context |
| Workflow still feels heavy | No new phase, artifact, reference, symlink, or script; compact Fast-Path |
| Plan v1 history causes ambiguity | Plan v2 explicitly supersedes v1 and uses a new approval commit |

## Documentation Impact

Confirmed narrow authority patches on plan v2 approval:

- `docs/authority/workflow/development-flow.md`: add the mandatory eligible
  post-execution review/fix loop and atomic commit closure behavior.
- `docs/authority/operations/git.md`: retain the title standard and add concise
  semantic atomic commit boundaries and authorization.
- `docs/authority/architecture/repo-and-skills.md`: change the assessment group
  description from manual review to read-only review plus manual challenge.

The constitution and Agent Policy remain unchanged.

## Plan Revision History

| Version | Status | Decision |
|---:|---|---|
| 1 | superseded before behavior implementation | Approved broad design with two shared references, symlinks, and verifier; owner rejected it as over-engineered. |
| 2 | draft | Inline review behavior in `review/SKILL.md`; stage-owned commit rules; no new references or verifier. |

## Confirmation Log

| When | Confirmed By | Plan Version | Confirmation |
|---|---|---:|---|
| 2026-07-10T16:28:08+08:00 | user | 1 | `批准。先提交，然后执行` |
| 2026-07-10T17:10:43+08:00 | user | 2 | `批准` |
