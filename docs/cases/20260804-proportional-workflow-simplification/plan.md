---
case_id: 20260804-proportional-workflow-simplification
plan_version: 1
status: approved
risk_level: high
approved_by: user
approved_at: 2026-08-04T15:33:56+08:00
base_commit: fd9de8de043b8831346604930fd13312efff8df8
---

# Implementation Plan — Proportional Workflow Simplification

## Human Approval Summary

- Outcome: make reversible one-turn low/medium work direct by default, keep a
  compact persistent case only when continuity or a durable decision is needed,
  and reserve the current full approval/review/closure rigor for guarded work.
- Material scope: change the active workflow, agent, and Git authority;
  synchronize the shared protocol, five development Skills, three artifact
  templates, and the current public/derived workflow descriptions.
- Decision needed: approve plan v1 as the current high-risk object. Approval
  removes the active Fast-Path concept and its three-file/twenty-line threshold,
  narrows reapproval and deep-review triggers, and stops requiring bookkeeping-
  only plan/closure commits for ordinary cases.
- Expected local Git actions / commit count: create branch
  `codex/proportional-workflow-simplification`, then create three local commits:
  approved plan, coherent implementation, and closure. Review fixes are added
  only when a finding needs an independently revertible correction.
- Likely interruptions: only a changed human outcome/non-goal, newly high-risk
  effect, public/authority conflict, dependency/config/publication request, or
  unresolved material review finding.
- Excluded actions: no Constitution amendment, new Skill/stage/artifact/risk
  level, historical-case migration, dependency/runtime/backend change, push,
  PR, merge, release, amend, rebase, squash, or history rewrite.

## Goal

Implement a proportional development workflow through two independent
questions instead of a mandatory pipeline:

1. Does the work need a durable case for continuity, a durable human decision,
   or guarded execution?
2. Does consequence, irreversibility, exposure, authority impact, or weak
   verification require guarded assurance?

The resulting behavior is:

- direct work: reversible, one-turn low/medium work uses normal repository
  execution, verification, and final reporting without case artifacts;
- compact persistent case: continuity or a durable decision uses a concise
  plan and closure, with execution evidence only when triggered;
- guarded case: high-risk/public/authority-sensitive or weakly evidenced work
  retains explicit current-plan approval, exact baseline, deep Engineering/Spec
  review, and durable closure evidence.

## Non-goals

- Do not weaken protection for security, auth, permission, privacy, billing,
  deletion, public compatibility, irreversible migration, L1 facts, or
  authorization-changing workflow policy.
- Do not remove TDD defaults or alternative verification for confirmed
  exceptions.
- Do not auto-trigger ad-hoc `review` or `grill`; selective review here refers
  only to workflow-owned Post-execution review.
- Do not remove existing Skill names or ownership boundaries.
- Do not merge the three case artifact types in this change; make them
  conditional and compact first.
- Do not rewrite historical cases, their commits, or the existing changelog
  entries that describe the old policy at the time.
- Do not regenerate the static usage-flow images; public captions will identify
  them as the guarded-case path, and current text/Mermaid will describe all
  paths.

## Context

- Intent: workflow / Skill simplification.
- Case: `20260804-proportional-workflow-simplification`.
- Route verdict: `proceed_to_plan`.
- Risk: high because the change deliberately relaxes authorization, review,
  artifact, and commit requirements.
- Current active cases: none.
- User evidence: the owner requested implementation after a read-only review of
  51 real `rp-pi-1` cases. Those cases contain 10,614 lines of case Markdown;
  39/47 plans are medium risk; none used Fast-Path; and ordinary cases often
  produced more case-document churn than product change.
- Historical evidence: the closed
  `20260712-human-first-workflow-simplification` case kept artifact consolidation
  as a follow-up if ceremony remained costly; the current real-case sample now
  supplies that evidence.
- Authority fit: Constitution minimum-path and human-first principles plus
  ADR-0002 require confirmation only at real decision boundaries. Current
  lower-level rules conflict in effect by routing medium work through a full
  case and making internal file/test discovery approval-sensitive.

Included sources:

| Source | Reason | Trust |
|---|---|---|
| `docs/authority/constitution.md` | Minimum path and human-first constraints. | Highest |
| `docs/authority/README.md` and relevant authority docs | Current workflow, risk, Git, and ownership facts. | High |
| `docs/adr/ADR-0002-human-first-agent-responsibility.md` | Accepted confirmation-boundary decision. | High |
| Current `skills/**` | Executable Agent Skill behavior. | High |
| Current-turn review of `rp-pi-1/docs/cases` and Git history | Direct usage and process-cost evidence. | Current external evidence |
| Prior simplification/atomic-review cases | Origins and intended follow-ups of current policy. | Historical evidence |

Excluded sources:

- Archive/raw designs: active authority and current implementation are
  sufficient; archive cannot override them.
- `rp-pi-1` business correctness: the sample is process evidence only.
- Static usage-flow images as authority: they are derived illustrations.

## Decisions

1. Remove `Fast-Path` from active workflow terminology and delete its LOC/file
   threshold. Do not replace it with another numeric shortcut.
2. Create a case only for continuity, a durable human decision, explicit user
   request, or guarded execution. Medium risk alone is not a case trigger.
3. A non-guarded persistent plan may record the current unambiguous execute
   request as approval without asking again. Guarded work still requires an
   explicit confirmation of the written current plan.
4. Approval binds an outcome envelope: goal, guardrails/non-goals, acceptance,
   escalation triggers, and any specifically protected boundary. Exact file
   lists, added tests, and ordinary internal implementation choices are planning
   evidence, not reapproval triggers while they remain inside the envelope.
5. Reapproval is required only for outcome/non-goal changes, risk escalation,
   authority/public contract change, new dependency/config/CI/schema effect,
   external resource or irreversible action, or another explicitly protected
   boundary.
6. `execution.md` is conditional on resume/continuity evidence, material
   deviation, meaningful failure/retry history, deep-review findings, or an
   explicit request. A normal behavior change or TDD cycle alone does not force
   it.
7. Workflow-owned deep Engineering/Spec review is required for guarded work,
   material deviations, weak verification, public/authority-sensitive changes,
   or explicit plan/user request. Other work receives the executor's compact
   acceptance/test/diff/scope completion check without persisted dual axes.
8. A review returns its complete current finding set. Known findings are fixed
   in the smallest coherent batches; commit boundaries follow independent
   validity/revertibility, not one commit per finding ID.
9. Local commits are governed by user/project intent and semantic value.
   Ordinary case bookkeeping does not require standalone plan or closure
   commits. A guarded plan may still declare them when durable approval and
   auditability justify the cost.
10. A complete closure is terminal from its artifact evidence unless the
    approved plan declared a required commit; only then does commit success
    gate terminal status and unqualified `Done`.

## Workspace Baseline

- Base commit: `fd9de8de043b8831346604930fd13312efff8df8`.
- Branch / planned run mode: `main` now; create
  `codex/proportional-workflow-simplification` / `branch` after approval.
- Dirty or existing changes: none before creating this plan.

## Risk Level

High. The files are reversible Markdown, but the change weakens existing
authorization and assurance requirements. Protection comes from an explicit
current-plan confirmation, exact baseline, complete active-contract sync,
semantic validation, and mandatory Post-execution Engineering/Spec review for
this case.

## TDD Applicability

- TDD Required: No.
- Exception category: pure documentation / Agent Skill text; the repository has
  no runtime workflow interpreter or automated behavior suite.
- Alternative verification:
  - capture pre-change stale-rule matches for case, Fast-Path, amendment,
    review, artifact, commit, and Done semantics;
  - run required/forbidden semantic assertions after editing;
  - parse modified YAML/frontmatter with Python only through `uv`;
  - validate every modified Skill with the available Skill validator;
  - verify all local Markdown links and shared symlinks;
  - recalculate active Skill word cost and check declared ceilings;
  - run `git diff --check`, inspect complete/cached/unstaged/untracked scope,
    and perform exact-baseline Post-execution Engineering/Spec review.

## Files to Change

Authority:

- `docs/authority/README.md`
- `docs/authority/architecture/repo-and-skills.md`
- `docs/authority/workflow/development-flow.md`
- `docs/authority/agent/policy.md`
- `docs/authority/operations/git.md`

Skill implementation and templates:

- `skills/_shared/references/shared-protocol.md`
- `skills/development/docloom-workflow/SKILL.md`
- `skills/development/docloom-workflow/references/loop-protocol.md`
- `skills/development/context-authority/SKILL.md`
- `skills/development/plan-confirm/SKILL.md`
- `skills/development/plan-confirm/templates/plan.md`
- `skills/development/tdd-execute/SKILL.md`
- `skills/development/tdd-execute/templates/execution.md`
- `skills/development/doc-sync-close/SKILL.md`
- `skills/development/doc-sync-close/templates/closure.md`
- `skills/assessment/review/SKILL.md`

Public and derived documentation:

- `README.md`
- `README_CN.md`
- `CHANGELOG.md`
- `skills/README.md`
- `docs/product/current-state.md`
- `docs/share/workflow-and-design.md`
- `docs/cases/README.md`

Case evidence:

- `docs/cases/20260804-proportional-workflow-simplification/plan.md`
- create `execution.md` because this guarded case requires review evidence
- create `closure.md`

## Acceptance Criteria

| Criterion | Planned verification |
|---|---|
| Active workflow contains no Fast-Path/LOC/file-count gate. | Scoped stale-rule search excluding history/cases/archive. |
| Medium risk alone no longer creates a case or requires a new confirmation. | Shared/router/authority semantic assertions. |
| Direct, compact persistent, and guarded outcomes follow the two-question model without a new stage or risk level. | Cross-owner contract inspection and Spec review. |
| Approval binds the outcome envelope; internal file/test discovery is adaptive. | Plan/execute wording and amendment-trigger assertions. |
| Execution evidence is conditional rather than mandatory for every behavior/TDD change. | Shared artifact policy, executor workflow/template. |
| Deep Post-execution review is selective but remains mandatory for guarded and authority-sensitive work. | Workflow, executor, review, and Done-gate assertions. |
| Review fixes may be coherently batched; finding count does not dictate commit count. | Executor/review contract inspection. |
| Ordinary work has no mandatory bookkeeping-only commits; declared guarded commits still gate closure. | Shared/Git/plan/close authority consistency. |
| High-risk, public, privacy, destructive, irreversible, and authorization-changing protections remain intact. | Positive guard search and Engineering review. |
| Public English/Chinese and detailed workflow docs describe the same proportional model. | Link/text inspection; usage image captioned as guarded path. |
| No Skill, stage, artifact type, risk level, dependency, backend, or lifecycle domain is added. | Tree/diff/manifest inspection. |
| Modified frontmatter, links, symlinks, formatting, and Skill word ceilings pass. | Validators, link checks, `find -L`, word count, diff checks. |

## Tasks

### Task 1: Replace the escalation kernel

**Files:** shared protocol; workflow, agent, Git, and architecture authority.

- [ ] Replace Fast-Path and medium-case rules with continuity and guarded-risk
  predicates.
- [ ] Define outcome-envelope authorization, conditional artifacts, selective
  review, optional semantic commits, and conditional commit-gated closure.
- [ ] Preserve every high-risk and publication/history safeguard.

### Task 2: Align stage owners and compact templates

**Files:** router/context/plan/execute/close/review Skills, loop protocol, and
plan/execution/closure templates.

- [ ] Route direct work outside durable case machinery and keep persistent
  identity creation conditional.
- [ ] Make plan details adaptive inside the approved envelope and narrow
  amendment triggers.
- [ ] Make execution/report/review/closure behavior proportional and remove
  duplicate default sections.

### Task 3: Synchronize current human-facing documentation

**Files:** README/README_CN, current state, workflow/design guide, Skill map,
changelog, and case dashboard.

- [ ] Explain the two questions and three outcomes in human language.
- [ ] Reframe existing static usage images as guarded-case illustrations rather
  than the universal normal path.
- [ ] Remove active prose that promises mandatory full-flow commits/review for
  ordinary work.

### Task 4: Validate, review, fix, and close

**Files:** complete approved target plus case execution/closure evidence.

- [ ] Run semantic, frontmatter, Skill, link, symlink, word-cost, diff, and Git
  scope verification.
- [ ] Invoke mandatory Post-execution Engineering/Spec review for this guarded
  authority-changing case.
- [ ] Fix all material findings in coherent independently valid batches,
  re-review, sync dashboard, and create the declared closure commit.

## Post-Execution Review

- Exact baseline: `fd9de8de043b8831346604930fd13312efff8df8`.
- Engineering: verify trigger reliability, authorization safety, risk
  escalation, optional artifact/commit state derivation, Skill resource links,
  frontmatter, Git isolation, and unnecessary complexity.
- Spec: compare every decision and acceptance criterion with the Constitution,
  ADR-0002, the current-turn review recommendations, and protected non-goals.
- Aggregate must pass before closure; missing material evidence cannot pass.

## Atomic Commits

After approval:

1. `docs: approve proportional workflow simplification plan` — current plan,
   trailer `Doc-Loom-Step: plan`.
2. `refactor: make workflow assurance proportional` — coherent active
   authority, Skill, template, and current-doc implementation, trailer
   `Doc-Loom-Step: task:proportional-workflow`.
3. `docs: close proportional workflow simplification case` — final execution,
   closure, and dashboard sync, trailer `Doc-Loom-Step: closure`.

All use `Doc-Loom-Case: 20260804-proportional-workflow-simplification` and
explicit staging. No publication or history rewriting is authorized.

## Confirmation Log

| When | Confirmed By | Plan Version | Confirmation |
|---|---|---:|---|
| 2026-08-04T15:33:56+08:00 | user | 1 | `执行` — confirms the immediately pending plan v1 and authorizes its declared branch and local commits. |
