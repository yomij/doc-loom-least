---
case_id: 20260712-human-first-workflow-simplification
plan_version: 1
status: approved
risk_level: high
approved_by: user
approved_at: 2026-07-12T01:30:16+08:00
base_commit: db766193faa195c2451f840093cb0933f3c7e7f2
---

# Implementation Plan

## Human Approval Summary

- Outcome: make Doc Loom feel like one human-facing assistant while retaining
  the existing internal skill ownership and safety boundaries.
- Scope: update workflow/agent/Git authority, shared protocols, six current
  skills or references, artifact templates, and the public README.
- User decisions: one approval of this plan; later interruption only for a
  material deviation, unresolved review finding, or concrete authority choice.
- Local Git actions after approval: create branch
  `codex/human-first-workflow-simplification`, then create three planned local
  commits: approved plan, coherent implementation, and closure. A material
  review finding may add one narrowly scoped `fix:` commit.
- Excluded: no push, PR, merge, tag, release, amend, rebase, squash, history
  rewrite, dependency change, runtime backend, new workflow stage, or new
  artifact type.

## Goal

Implement the human-user review findings as one coherent simplification pass:

1. Make `docloom-workflow` the natural-language default entry for persistent
   feature, bug, refactor, workflow, status, and discovery requests.
2. Keep stage skills callable but hide internal routing names and state jargon
   from default human-facing responses.
3. Turn Fast-Path into a genuinely compact persistent flow with a minimal plan,
   optional execution report, compact review, and one combined implementation
   and closure commit instead of separate plan/task/closure commits.
4. Make local Git effects prominent before approval, including expected commit
   count and explicit exclusions such as push and history rewriting.
5. Refine risk classification so reversible internal workflow/skill text is not
   automatically treated like destructive security, billing, data, or public
   compatibility work.
6. Let next-slice discovery infer discoverable product-state facts and return
   provisional candidates; block only when a missing human fact would materially
   change the recommendation.
7. Put a short human summary first in case artifacts and reduce repetition
   between plan, execution, and closure.
8. Make `grill` use A/B/C only for genuinely discrete decisions and allow
   free-form answers for exploratory questions.

## Non-goals

- Do not amend the Constitution or weaken its confirmation and human-first
  principles.
- Do not remove or rename existing skills, ownership boundaries, case identity,
  status-only safety, TDD defaults, Post-execution review, or authority rules.
- Do not weaken high-risk handling for security, auth, permission, privacy,
  billing, data deletion, destructive migration, or public compatibility.
- Do not introduce a CLI backend, daemon, app backend, scheduler, queue, or
  centralized orchestration layer.
- Do not add workflow phases, artifact types, risk levels, or lifecycle domains.
- Do not rewrite historical cases to the new compact format.
- Do not require users to know internal stage skill names.

## Context

- Intent: workflow / skill design.
- Route Verdict: `proceed_to_plan`.
- Proposed case slug: `20260712-human-first-workflow-simplification`.
- User request: all findings from the human-user review are valuable; begin
  addressing them.
- Review conclusion: the internal governance model is strong, but the public
  experience exposes too many skills, stages, artifacts, risk labels, and Git
  mechanics.
- Active cases: none before this case.
- Authority fit: the Constitution and ADR-0002 require minimum-path,
  human-semantic interaction and agent-owned bookkeeping. The requested changes
  strengthen those principles rather than changing them.

Context sources:

| Source | Why Included | Trust |
|---|---|---|
| `docs/authority/constitution.md` | Minimum path and human-first constraints. | Highest |
| `docs/authority/README.md` | Authority order and current workflow policy entry. | High |
| `docs/authority/workflow/development-flow.md` | Current stage, confirmation, review, commit, and Done policy. | High |
| `docs/authority/agent/policy.md` | Agent responsibility and current high-risk topic definition. | High |
| `docs/authority/operations/git.md` | Current case commit and authorization policy. | High |
| `docs/adr/ADR-0001-lifecycle-scope-and-skill-grouping.md` | Stable skill grouping and invocation names. | High |
| `docs/adr/ADR-0002-human-first-agent-responsibility.md` | Owner-confirmed human-first decision. | High |
| Current `skills/**` | Observable workflow implementation. | High |
| Existing dogfood cases | Evidence of artifact and commit volume. | Historical evidence |
| Current-turn user review acceptance | Authorization to prepare a concrete plan. | Pending plan approval |

Excluded context:

- Archived design material: the active authority and current skills are
  sufficient and no historical conflict is present.
- Runtime code/tests: this repository has no workflow interpreter or runtime
  source tree; Skill Markdown is the implementation contract.

## Decisions

1. Preserve all current skill names, but make `docloom-workflow` the only
   recommended public doorway. Advanced users may still invoke stage skills.
2. Default conversation output uses human concepts: status, completed work,
   next action, decision needed, and Git effect. Internal phase/verdict/skill
   names appear only when useful for diagnosis or requested detail.
3. Fast-Path keeps existing artifact types but uses only `case_state.yaml`, a
   short `plan.md`, and a short `closure.md` by default. `execution.md` remains
   optional. The green change, compact review evidence, closure, and state are
   committed once as a combined compact completion unit; no separate plan or
   closure commit is required.
4. Full eligible cases retain semantic atomic commits and dual-axis review.
   Every approval prompt must surface the expected local Git actions and state
   that push/history rewriting are excluded.
5. Keep `low`/`medium`/`high`, but classify using consequence,
   reversibility, exposure, and authority impact. Reversible internal workflow
   text is normally medium; a change that weakens authorization, changes a
   public contract, or enables irreversible action remains high.
6. Workflow and agent policy remain mandatory `context-authority` triggers even
   when the final execution risk is medium.
7. Discovery may infer product-state fields from active authority, current
   implementation, README, case follow-ups, and current-turn facts. Inferred
   facts must be labeled; only material ambiguity blocks ranking.
8. Artifact templates lead with a short human summary and reference prior
   evidence instead of duplicating full tables or narratives.
9. `grill` remains one-question-at-a-time, but options are conditional rather
   than mandatory.

## Workspace Baseline

- Base Commit: `db766193faa195c2451f840093cb0933f3c7e7f2`
- Branch Before Planning: `main`
- Planned Run Mode: `branch`
- Planned Branch: `codex/human-first-workflow-simplification`
- Dirty Workspace Before Case: no
- Existing Changed Files Before Case: none

## Risk Level

High under the current policy because this case changes workflow and agent
policy, confirmation semantics, Fast-Path commit behavior, and active authority.
The implementation is reversible Markdown, but the current risk rules remain
authoritative until this plan is executed and reviewed.

Key protection: no security/data/public-contract safeguards are relaxed, and
the new risk wording must keep authorization-changing workflow edits high.

## TDD Applicability

- TDD Required: No.
- Exception: pure Markdown Skill, authority, template, and README changes; no
  runtime workflow interpreter exists.
- Characterization lock:
  - capture current skill descriptions, Fast-Path artifact/commit contract,
    high-risk wording, discovery blocking rule, output contracts, and grill
    question format before editing;
  - preserve negative guards for no push/history rewrite, no automatic ad-hoc
    review/grill, no authority promotion, and no new pipeline machinery.
- Alternative Verification:
  - targeted positive and stale-rule `rg` checks;
  - `git diff --check` and staged-diff inspection;
  - YAML/frontmatter parsing;
  - `uv run --with pyyaml python .../quick_validate.py` for every modified Skill;
  - link/symlink validation;
  - complete-delta Engineering and Spec Post-execution review.

## Post-Execution Review Strategy

- Exact baseline: approved `base_commit` above.
- Engineering axis: check trigger reliability, routing safety, risk and Git
  authorization boundaries, Fast-Path state/commit coherence, artifact
  deduplication, links, frontmatter, and unnecessary complexity.
- Spec axis: compare the final delta with the eight Goal items, Decisions,
  Non-goals, the accepted human-user review findings, Constitution, and
  ADR-0002.
- Critical/Important findings return to one smallest coherent `fix:` commit and
  affected-axis re-review. Missing material evidence cannot pass.
- Durable evidence: `execution.md` and `closure.md`; no `review.md`.

## Atomic Commit Strategy

Expected local commits after plan approval:

| Step | Commit Title | Trailer |
|---|---|---|
| Approved plan | `docs: approve human-first workflow simplification plan` | `Doc-Loom-Step: plan` |
| Coherent implementation | `refactor: simplify the human-facing Doc Loom workflow` | `Doc-Loom-Step: task:human-first-workflow` |
| Review fix, only if needed | `fix: <review root cause>` | `Doc-Loom-Step: review-fix:<id>` |
| Closure | `docs: close human-first workflow simplification case` | `Doc-Loom-Step: closure` |

All commits use
`Doc-Loom-Case: 20260712-human-first-workflow-simplification`, explicit path
staging, staged-diff inspection, `git diff --cached --check`, and relevant
validation. Plan approval authorizes only these local case commits and planned
branch creation. It does not authorize push, PR, merge, tag, release, amend,
rebase, squash, history rewriting, or unrelated files.

## Files to Change

Authority and public entry:

- `docs/authority/architecture/repo-and-skills.md`
- `docs/authority/workflow/development-flow.md`
- `docs/authority/agent/policy.md`
- `docs/authority/operations/git.md`
- `README.md`
- `skills/README.md`

Shared behavior:

- `skills/_shared/references/shared-protocol.md`
- `skills/_shared/references/loop-protocol.md`

Skills and references:

- `skills/development/docloom-workflow/SKILL.md`
- `skills/development/plan-confirm/SKILL.md`
- `skills/development/plan-confirm/references/risk-levels.md`
- `skills/development/tdd-execute/SKILL.md`
- `skills/development/doc-sync-close/SKILL.md`
- `skills/assessment/grill/SKILL.md`

Templates:

- `skills/development/plan-confirm/templates/plan.md`
- `skills/development/tdd-execute/templates/execution.md`
- `skills/development/doc-sync-close/templates/closure.md`

Case evidence and derived dashboard:

- `docs/cases/20260712-human-first-workflow-simplification/case_state.yaml`
- `docs/cases/20260712-human-first-workflow-simplification/plan.md`
- create `docs/cases/20260712-human-first-workflow-simplification/execution.md`
- create `docs/cases/20260712-human-first-workflow-simplification/closure.md`
- `docs/cases/README.md`

## Acceptance Criteria

| Criteria | Planned Verification |
|---|---|
| Ordinary persistent feature, bug, refactor, and workflow requests can trigger the default entry without naming a stage skill. | Inspect frontmatter and README invocation examples; semantic `rg`. |
| Default status/routing output is human-readable and does not require phase, verdict, or next-skill jargon. | Inspect `docloom-workflow` output contracts and README examples. |
| Fast-Path defaults to state + short plan + short closure, optional execution evidence, compact review, and one combined completion commit. | Inspect shared protocol, plan/execute/close skills, templates, and authority; stale-rule `rg`. |
| Full eligible cases retain dual-axis review, semantic commits, closure evidence, and clean-worktree Done protection. | Positive semantic checks and Engineering review. |
| Approval prominently states expected local Git actions and excludes push/history rewriting. | Inspect plan skill/template, shared protocol, Git authority, and this case confirmation surface. |
| Reversible internal workflow/skill text can be medium risk, while authorization weakening and irreversible/public-impact changes remain high. | Inspect agent authority, shared protocol, risk levels, context and plan skills. |
| Missing discoverable product-state fields are inferred and labeled; only material ambiguity blocks candidates. | Inspect loop protocol and router behavior; positive/stale-rule `rg`. |
| Plan, execution, and closure lead with concise human summaries and avoid copying evidence already owned elsewhere. | Inspect templates and stage skill artifact rules. |
| Grill uses options only for discrete decisions and permits free-form exploratory answers. | Inspect `grill/SKILL.md`. |
| No skill names, stages, artifact types, risk levels, runtime components, or lifecycle domains are added or removed. | Tree diff, frontmatter validation, and Spec review. |
| Modified Skills and YAML/frontmatter remain valid; links and shared reference symlinks resolve. | Quick validator, YAML parse, broken-link/symlink checks, `git diff --check`. |

## Tasks

### Task 1: Create one human-facing doorway

**Files:** `docloom-workflow/SKILL.md`, `README.md`, `skills/README.md`,
`repo-and-skills.md`, `development-flow.md`.

- [x] Broaden the default-entry trigger without hijacking explanation-only,
  one-shot review, or explicit stage-skill requests.
- [x] Replace default route jargon with human status/action/decision language.
- [x] Present stage skills as internal/advanced entry points in public docs.

### Task 2: Make Fast-Path compact and Git authorization visible

**Files:** shared protocol, plan/execute/close skills, plan/closure templates,
Git and workflow authority.

- [x] Define one combined compact completion commit and remove separate
  Fast-Path plan/closure commit requirements.
- [x] Keep full-case Post-execution review and semantic commit safety unchanged.
- [x] Add a prominent approval summary with expected local Git actions and
  excluded publication/history actions.

### Task 3: Refine risk without weakening dangerous-change protection

**Files:** agent policy, shared protocol, risk levels, context/plan wording.

- [x] Base risk on impact, reversibility, exposure, and authority consequences.
- [x] Keep workflow/agent policy as a mandatory context trigger.
- [x] Keep authorization weakening, irreversible action, public compatibility,
  security, billing, privacy, permission, deletion, and destructive migration
  high risk.

### Task 4: Improve discovery and human evidence density

**Files:** loop protocol, router, plan/execution/closure templates and skills.

- [x] Infer discoverable current-state fields and label assumptions/confidence.
- [x] Block only on material decision ambiguity.
- [x] Put concise human summaries first and replace repeated evidence with
  stable references.

### Task 5: Relax grill's interaction shape

**Files:** `skills/assessment/grill/SKILL.md`.

- [x] Preserve one high-leverage question at a time.
- [x] Use options only for genuinely discrete decisions; allow open responses.
- [x] Retain fact verification and non-promotion safeguards.

### Task 6: Validate, review, and close

**Files:** case execution/closure/state and derived dashboard.

- [x] Run all alternative verification and inspect the complete delta.
- [x] Run isolated Engineering and Spec Post-execution review.
- [x] Fix and re-review any material finding.
- [x] Sync authority/public documentation and create the declared closure commit.

## Risks

| Risk | Mitigation |
|---|---|
| A broader entry description could trigger Doc Loom for trivial questions. | Preserve explicit exclusions for explanation-only, one-shot review, and low-risk local edits. |
| Compact Fast-Path could weaken evidence or produce an invalid mixed commit. | Limit it to existing low-risk/small/no-continuity conditions and require the single completion unit to be green, reviewed, and independently revertible. |
| Lowering reversible workflow text to medium could hide authorization risk. | Keep any authorization weakening, irreversible action enablement, public contract, or L1 critical fact high. |
| Human summaries could omit evidence needed for recovery. | Keep detailed evidence when material, but reference it once from the owning artifact. |
| Many coordinated Markdown contracts could drift. | One coherent implementation commit, targeted stale-rule checks, validator runs, and mandatory dual-axis review. |

## Documentation Impact

- Applies confirmed narrow patches to existing workflow, agent, Git, and
  repo/skill authority documents.
- Updates the public README and skill grouping map.
- Does not amend the Constitution, add a governance batch, or create a new
  authority area.

## Confirmation Log

| When | Confirmed By | Plan Version | Confirmation |
|---|---|---|---|
| 2026-07-12T01:30:16+08:00 | user | 1 | "批准计划" |
