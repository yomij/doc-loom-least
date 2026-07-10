---
case_id: 20260710-compress-workflow-skill-text
plan_version: 1
status: approved
risk_level: high
approved_by: user
approved_at: 2026-07-10T18:01:15+08:00
base_commit: 1d5a05b2e3a49637876447c520a917e6e80dd31e
---

# Implementation Plan

## Goal

Compress the seven workflow and authority contracts introduced or expanded by
the previous case while preserving their observable behavior, ownership,
gates, authorization boundaries, and evidence requirements.

Target: reduce the seven files from 1,610 lines / 9,854 words to at most 1,355
lines / 8,600 words, with no semantic regression. Further reduction is welcome
only when readability stays high.

## Non-goals

- Do not change review, commit, routing, TDD, closure, or authority behavior.
- Do not add or split content into new references, scripts, symlinks, phases,
  artifacts, or helper Skills.
- Do not modify the Constitution, authority index, architecture authority,
  README files, templates, archived cases, or the prior closed case.
- Authority compression is limited to `development-flow.md` and `git.md` and
  must retain sufficient principle-level statements rather than implementation
  procedure.
- Do not use sub-agents unless the owner explicitly requests them.
- Do not push, merge, tag, release, amend, rebase, or squash history.

## Context

- User request: reduce Skill text growth after the mandatory review/atomic
  commit implementation.
- Route Verdict: `proceed_to_plan`.
- Authority: the Constitution requires the narrowest, clearest path and rejects
  duplicate machinery; current behavior remains authoritative.
- Baseline commit: `1d5a05b2e3a49637876447c520a917e6e80dd31e`.
- Prior case is closed; this is a separate behavior-preserving refactor.
- A second proposed plan correctly identified that the largest repository-wide
  duplication also exists in authority docs.
- Owner decision: combine Skill/shared and authority compression in this case,
  while preserving separate atomic task boundaries and review evidence.

Ponytail findings:

- `review/SKILL.md:L78-L303: shrink` Post-execution preflight, axes, gate, and
  two output contracts repeat shared Standard review concepts. Keep one compact
  mode contract plus the existing finding format.
- `tdd-execute/SKILL.md:L175-L292: shrink` review and commit policy repeats
  `review`, shared protocol, and Git authority. Keep invocation, result handling,
  stage transitions, and one commit checklist.
- `doc-sync-close/SKILL.md:L15-L103,L141-L190: shrink` closure evidence and
  commit gates recur across Inputs, Workflow, State, and Gates. Consolidate into
  one Done gate and one closure-commit rule.
- `plan-confirm/SKILL.md:L35-L169: shrink` requirements/plan commits,
  authorization exclusions, and confirmation rules are repeated. Keep one
  ordered workflow and one authorization paragraph.
- `shared-protocol.md:L171-L258: shrink` review/commit details exceed its
  cross-skill role. Keep ownership, compatibility, degraded Git, and shared
  authorization/closure facts; leave stage procedure to owners.
- `development-flow.md:L15-L157: shrink` the ownership table and detailed
  Post-execution/atomic sections restate current Skills and shared protocol.
  Replace them with a pointer plus four workflow principles.
- `git.md:L40-L84: shrink` atomic definition, exclusion list, and history
  rewrite procedure repeat shared and `tdd-execute`. Keep title rules, trailer
  syntax, and concise Git principles.

Combined estimate: `net: -240 to -300 lines possible.`

## Decisions

1. `review` uniquely owns the dual-axis contract, aggregate gate table, finding
   format, and read-only gate.
2. `shared-protocol` uniquely owns the full authorization exclusion list,
   cross-skill Atomic Commit Contract, ownership map, compatibility, and shared
   degraded-Git/closure facts.
3. `tdd-execute` keeps eligibility, invocation timing, fix/re-review ownership,
   stage transitions, and commit procedure; it points to `review` and shared
   protocol instead of restating their tables and exclusion lists.
4. `plan-confirm` keeps approval recording plus requirements/plan commit
   ownership and points to shared authorization boundaries.
5. `doc-sync-close` keeps one consolidated Done gate and one closure-commit
   rule instead of repeating them across Inputs, Workflow, State, and Gates.
6. Existing references remain; no new reference hides the growth. Frontmatter
   descriptions retain all trigger semantics.
7. `development-flow.md` points to shared ownership and retains four principles:
   eligible cases require Post-execution review; semantic commits occur at
   completion points; plan approval excludes push/history rewriting; `Done`
   requires passing review and a successful closure commit.
8. `git.md` retains commit-title rules, trailer syntax, atomicity as a principle,
   and the fact that rewritten history invalidates evidence. Procedure belongs
   to `tdd-execute` and shared protocol.
9. Per-file targets are guidance, not hard density quotas: review ~260,
   execute ~270, close ~180, plan ~180, shared ~300, development-flow ~115,
   git ~50. The required check is total budget, readability, and semantics.
10. Compression is accepted only when characterization and final
    Engineering/Spec review pass.

## Workspace Baseline

- Proposed Base Commit: `1d5a05b2e3a49637876447c520a917e6e80dd31e`
- Branch: `case/20260710-compress-workflow-skill-text`
- Run Mode: `branch`
- Dirty Workspace Before Case: no
- Current Changes: this draft plan and routing state only

## Risk Level

High: the edit is intended to be behavior-preserving, but it rewrites workflow
policy contracts. Accidental omission could weaken review, commit, or closure
gates. Explicit confirmation of this plan is required.

## TDD Applicability

- TDD Required: No.
- Exception: Pure documentation change; behavior-preserving refactor.
- Characterization: capture current line/word counts, required semantic anchors,
  frontmatter, and negative stale-rule checks before editing.
- Alternative Verification: before/after semantic searches, Skill validation,
  YAML parsing, line/word budget, diff inspection, Git history checks, and final
  Post-execution Engineering/Spec review.

## Post-Execution Review Strategy

- Exact baseline: approved `base_commit`.
- Engineering: check trigger clarity, owner boundaries, internal consistency,
  Git/closure safety, Skill validity, and whether more text can be deleted.
- Spec: compare the final diff with this plan and the prior case requirements,
  ensuring no review/commit/closure behavior was lost.
- Critical/Important findings return to a verified atomic fix commit and
  re-review; missing evidence cannot pass.
- Durable evidence: `execution.md` and `closure.md`; no `review.md`.

## Atomic Commit Strategy

| Step | Commit Title | Trailer |
|---|---|---|
| Approved plan | `docs: approve workflow skill compression plan` | `Doc-Loom-Step: plan` |
| Authority contracts | `refactor: compress workflow and Git authority` | `Doc-Loom-Step: task:authority-contracts` |
| Shared contract | `refactor: compress shared workflow contract` | `Doc-Loom-Step: task:shared-contract` |
| Skill contracts | `refactor: compress review and stage skill contracts` | `Doc-Loom-Step: task:skill-contracts` |
| Review fix, if needed | Exact `fix:` title per root cause | `Doc-Loom-Step: review-fix:F1`, then increment |
| Closure | `docs: close workflow skill compression case` | `Doc-Loom-Step: closure` |

All commits use `Doc-Loom-Case: 20260710-compress-workflow-skill-text`, explicit
staging, staged-diff inspection, and relevant passing checks.

## Files to Change

Behavior-preserving compression:

- `skills/assessment/review/SKILL.md`
- `skills/development/tdd-execute/SKILL.md`
- `skills/development/doc-sync-close/SKILL.md`
- `skills/development/plan-confirm/SKILL.md`
- `skills/_shared/references/shared-protocol.md`
- `docs/authority/workflow/development-flow.md`
- `docs/authority/operations/git.md`

Case evidence:

- `docs/cases/20260710-compress-workflow-skill-text/plan.md`
- `docs/cases/20260710-compress-workflow-skill-text/case_state.yaml`
- create `docs/cases/20260710-compress-workflow-skill-text/execution.md`
- create `docs/cases/20260710-compress-workflow-skill-text/closure.md`
- `docs/cases/README.md` at closure

## Acceptance Criteria

| Criteria | Planned Verification |
|---|---|
| The seven target files total at most 1,355 lines; no file grows. Soft targets are review 260, execute 270, close 180, plan 180, shared 300, development-flow 115, git 50. | `wc -l` per file and total. |
| Total target words decrease materially from 9,854. | `wc -w`; target at most 8,600. |
| Frontmatter names/descriptions and all trigger paths remain valid. | YAML parse and Skill `quick_validate.py`. |
| Exact baseline and complete-delta review semantics remain. | Positive semantic `rg` and direct inspection of `review`. |
| Engineering/Spec isolation, aggregate results, finding severity, and read-only boundary remain. | Positive semantic `rg`; final Spec review against prior requirements. |
| `tdd-execute` still owns invocation, fix/re-review, task/fix commits, rewritten-history refresh, and `doc_syncing` transition only after pass. | Targeted inspection and semantic anchors. |
| `plan-confirm` still owns requirements/plan commits, confirmation, strategies, and scoped authorization. | Targeted inspection and semantic anchors. |
| `doc-sync-close` still requires acceptance, passing review, required commits, successful closure commit, and clean scope before `Done`. | Targeted inspection and semantic anchors. |
| Shared compatibility, Fast-Path, Git degraded mode, state derivation, and authorization facts remain. | Targeted inspection and semantic anchors. |
| No new reference, script, symlink, phase, or artifact type is introduced. | Added-file, path, and broken-symlink checks. |
| Workflow authority retains the four required principles and points to current owners instead of restating procedure. | Direct authority inspection plus final Spec review. |
| Git authority retains title/trailer rules, atomicity, closure evidence, and rewritten-history invalidation without stage procedure. | Direct authority inspection plus final Spec review. |
| All other authority and derived docs remain unchanged. | Final changed-file list. |
| Final Engineering and Spec review aggregate passes. | Record verdicts in `execution.md`. |

## Tasks

### Task 0: Commit the approved compression plan

**Files:** `plan.md`, `case_state.yaml`

- [x] Write approval fields, confirmation log, and planned state.
- [x] Validate YAML/frontmatter and staged scope.
- [x] Create the declared atomic plan commit.

### Task 1: Characterize and compress authority contracts

**Files:** `docs/authority/workflow/development-flow.md`,
`docs/authority/operations/git.md`, and case evidence.

- [x] Record current authority claims and line/word counts.
- [x] Replace duplicated ownership/procedure with pointers and concise
  principle-level statements.
- [x] Verify the four workflow principles, Git title/trailer contract,
  rewritten-history invalidation, YAML/frontmatter, links, and diff.
- [x] Create the declared atomic authority-contract commit.

### Task 2: Compress the shared contract

**Files:** `skills/_shared/references/shared-protocol.md`, `plan.md`,
`case_state.yaml`, and `execution.md`.

- [x] Record current counts and semantic characterization.
- [x] Deduplicate ownership prose, authorization exclusions, Atomic Commit
  wording, and shared closure facts without removing shared semantics.
- [x] Run line/word budgets, semantic positive/negative checks, Skill
  consumer-path inspection, YAML, symlink, and diff checks.
- [x] Create the declared atomic shared-contract commit.

### Task 3: Compress Review and stage Skills

**Files:** `review`, `tdd-execute`, `plan-confirm`, `doc-sync-close`, plus case
evidence.

- [x] Keep complete rules only at the approved owner and replace consumer
  restatements with short references and local gates.
- [x] Preserve frontmatter triggers, all stage ownership, and prior-case
  behavioral anchors.
- [x] Run Skill validation, total budgets, semantic positive/negative checks,
  YAML, symlink, and diff checks.
- [x] Create the declared atomic Skill-contract commit.

### Task 4: Run mandatory review and fix loop

- [x] Run separate Engineering and Spec passes against the complete delta.
- [x] Fix each material root cause in an atomic commit and re-review.
- [x] Record final per-axis and aggregate verdicts.

### Task 5: Close with the atomic closure commit

- [x] Map acceptance criteria to final evidence.
- [x] Write closure, closed state, and dashboard update without implementation
  fixes.
- [x] Create and verify the closure commit; report `Done` only when clean.

## Planned Commands

```text
wc -l -w skills/assessment/review/SKILL.md skills/development/tdd-execute/SKILL.md skills/development/doc-sync-close/SKILL.md skills/development/plan-confirm/SKILL.md skills/_shared/references/shared-protocol.md docs/authority/workflow/development-flow.md docs/authority/operations/git.md
rg -n 'Post-execution|Engineering|Spec|base_commit|changes_required|insufficient_evidence|requirements commit|plan commit|review-fix|history rewrite|closure commit|Git Degraded|Fast-Path|Doc-Loom-Step|Git Commit Titles' skills/assessment/review/SKILL.md skills/development/tdd-execute/SKILL.md skills/development/doc-sync-close/SKILL.md skills/development/plan-confirm/SKILL.md skills/_shared/references/shared-protocol.md docs/authority/workflow/development-flow.md docs/authority/operations/git.md
rg -n 'reviews are always manual|Review 始终是手动行为|Default: do not stage or commit\.|Missing review conversation is not by itself a closure blocker\.' skills/assessment/review/SKILL.md skills/development/tdd-execute/SKILL.md skills/development/doc-sync-close/SKILL.md skills/development/plan-confirm/SKILL.md skills/_shared/references/shared-protocol.md
for skill_dir in skills/assessment/review skills/development/tdd-execute skills/development/doc-sync-close skills/development/plan-confirm; do uv run --with pyyaml python /Users/yomi/.codex/skills/.system/skill-creator/scripts/quick_validate.py "$skill_dir"; done
ruby -e 'require "yaml"; %w[skills/assessment/review/SKILL.md skills/development/tdd-execute/SKILL.md skills/development/doc-sync-close/SKILL.md skills/development/plan-confirm/SKILL.md docs/cases/20260710-compress-workflow-skill-text/plan.md].each { |path| text = File.read(path); fm = text.match(/\A---\n(.*?)\n---\n/m)&.[](1) or abort(path); YAML.safe_load(fm, permitted_classes: [Time], aliases: false) }; YAML.safe_load(File.read("docs/cases/20260710-compress-workflow-skill-text/case_state.yaml"), permitted_classes: [Time], aliases: false)'
find -L skills -type l -print
git diff --check
git diff --cached --check
git diff --name-only 1d5a05b2e3a49637876447c520a917e6e80dd31e
git log --format='%H%n%B%n---' 1d5a05b2e3a49637876447c520a917e6e80dd31e..HEAD
```

## Risks

| Risk | Mitigation |
|---|---|
| Shorter prose silently drops a gate | Characterization anchors plus Spec review against prior requirements |
| Compression merely moves text elsewhere | No new references or helper files; changed-file gate |
| Line target harms readability | Only the total budget is required; per-file values are soft targets |
| Ownership becomes ambiguous | One owner states full behavior; each consumer states its exact trigger/result |
| Authority becomes too thin | Keep and verify the four workflow principles plus Git title/trailer/evidence facts |

## Documentation Impact

Confirmed narrow authority patches on plan approval:

- `docs/authority/workflow/development-flow.md`: remove duplicated ownership
  and procedure; retain owner pointer and four mandatory workflow principles.
- `docs/authority/operations/git.md`: retain title/trailer standards and concise
  atomic, closure-evidence, and rewritten-history principles; remove stage
  procedure and the duplicated full exclusion list.

No other authority or derived documentation changes.

## Confirmation Log

| When | Confirmed By | Plan Version | Confirmation |
|---|---|---:|---|
| 2026-07-10T18:01:15+08:00 | user | 1 | `批准执行` |
