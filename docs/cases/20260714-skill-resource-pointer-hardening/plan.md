---
case_id: 20260714-skill-resource-pointer-hardening
plan_version: 1
status: approved
risk_level: medium
approved_by: user
approved_at: 2026-07-14T20:05:17+0800
base_commit: b0505d15c0cae72740f2a3e54a0c48002db13b36
---

# Implementation Plan

## Human Approval Summary

- Outcome: every canonical Doc Loom Skill resolves local references and
  templates predictably from its own `SKILL.md` directory, using explicit
  Markdown links instead of ambiguous bare path prose.
- Material scope: harden local resource pointers in the seven canonical Skills
  that currently load references/templates, record the authoring convention in
  `skills/README.md`, and verify link, symlink, copy-install, and unchanged
  workflow semantics.
- Decision needed: approve plan version 1 before any canonical Skill or loading
  guidance is changed.
- Expected local Git actions / commit count: create/switch to
  `codex/skill-resource-pointer-hardening`; normally 3 local commits for the
  approved plan, verified Skill change, and closure. Material review findings
  may add scoped `fix:` commits.
- Likely interruptions: the repository has no executable workflow interpreter,
  so verification is structural and semantic. The active Skillshare clone is
  nine commits behind this development repository and will not receive this
  change without separately authorized push/update/sync work.
- Excluded actions: no shared-protocol content change, symlink topology change,
  workflow stage or authorization change, dependency/script/CI addition,
  absolute machine path, installed-cache edit, push, PR, merge, release, or
  history rewrite.

## Goal

Remove the recurring resource-base ambiguity that causes agents to read paths
such as `references/shared-protocol.md` from `skills/development/` or another
guessed directory instead of from the directory containing the active
`SKILL.md`.

The durable authoring contract is:

1. each Skill states that local resource links resolve from its own `SKILL.md`
   directory;
2. each resource is named by a direct Markdown link beginning with
   `./references/` or `./templates/`;
3. trigger wording states when the resource must be read and before which
   action;
4. shared canonical files remain single-source resources exposed through the
   existing local symlinks.

## Non-goals

- Do not copy or inline `shared-protocol.md` into consumer Skills.
- Do not replace or relocate existing shared symlinks or private resources.
- Do not change routing, authority order, status derivation, risk
  classification, confirmation, TDD, review, governance, closure, or Git
  behavior.
- Do not make individual Skills independently installable outside the supported
  repository-wide Skillshare distribution.
- Do not add a permanent validator, runtime, package manifest, or test suite for
  this wording-only defect.
- Do not publish or synchronize the change into the active installed copy
  without separate authorization.

## Context

- Summary: the user reports repeated agent failures resolving local Skill
  references. This conversation reproduced the exact failure: the agent read
  `skills/development/references/shared-protocol.md`, omitting the owning
  `docloom-workflow/` directory. Plain `rg --files` then hid the valid symlink
  and encouraged an incorrect fallback to `_shared`. Direct `test -e`, `ls`,
  and `realpath` checks prove both the development and installed copies have
  valid Skill-local resources. The defect is therefore a weak context pointer,
  not a broken filesystem contract.
- Route verdict: `proceed_to_plan`. Active constitution, repository/Skill
  architecture, development workflow, agent policy, distribution authority,
  current Skill implementation, and the user's repeated runtime evidence are
  consistent. No active case or blocking authority conflict exists.
- Included sources:
  - `docs/authority/constitution.md`: minimum-path and human-first constraints.
  - `docs/authority/architecture/repo-and-skills.md`: progressive disclosure,
    local resource surfaces, shared-contract ownership, and single-source
    semantics.
  - `docs/authority/workflow/development-flow.md`: current Skill/stage behavior
    that must remain unchanged.
  - `docs/authority/agent/policy.md`: workflow/Skill text is medium risk and
    requires this context check.
  - `docs/authority/operations/distribution.md`: shared resources remain
    exposed through local relative symlinks under repository-wide Skillshare
    installation.
  - Current `skills/**/SKILL.md`, local resources, and symlinks: direct
    implementation evidence.
  - Current-turn failure trace and user report: pending runtime evidence that
    the existing pointer wording is not predictably executable.
- Excluded sources: archived designs, unrelated closed cases, speculative new
  distribution modes, and the stale installed clone as authority for current
  source behavior.

## Workspace Baseline

- Base commit: `b0505d15c0cae72740f2a3e54a0c48002db13b36` on `main`.
- Branch / run mode: current `main`; planned full-flow `branch` mode on
  `codex/skill-resource-pointer-hardening` after approval.
- Dirty or existing changes: clean before case creation. This draft plan and
  its derived dashboard entry are the only case-scoped changes.

## Risk Level

Medium. The change is reversible Markdown-only Skill behavior, but it touches
seven canonical agent instruction surfaces and the shared authoring convention.
An incorrect rewrite could suppress a conditional read, load unnecessary
context, or alter workflow meaning. Exact pointer inventory, semantic
comparison, path checks, dereferenced-copy simulation, and Post-execution
Engineering/Spec review control the risk.

## TDD Applicability

- TDD Required: No.
- If No, reason and alternative verification: this is an allowed Skill/docs
  change with no executable workflow interpreter. Use
  `characterize -> change -> verify`: capture all existing local resource
  pointers and the reproduced wrong-base failure; rewrite only pointer form;
  then assert every pointer is explicit, every target resolves from its owning
  Skill, all symlinks and dereferenced copies remain readable, and workflow
  semantic anchors are unchanged.

## Files to Change

Canonical Skill pointers:

- `skills/development/docloom-workflow/SKILL.md`
- `skills/development/context-authority/SKILL.md`
- `skills/development/plan-confirm/SKILL.md`
- `skills/development/tdd-execute/SKILL.md`
- `skills/development/doc-sync-close/SKILL.md`
- `skills/governance/setup-doc-governance/SKILL.md`
- `skills/assessment/review/SKILL.md`

Authoring guidance:

- `skills/README.md`

Case evidence and derived dashboard:

- `docs/cases/20260714-skill-resource-pointer-hardening/plan.md`
- `docs/cases/20260714-skill-resource-pointer-hardening/execution.md`
- `docs/cases/20260714-skill-resource-pointer-hardening/closure.md`
- `docs/cases/README.md`

## Acceptance Criteria

| Criterion | Planned verification |
|---|---|
| Every canonical Skill-local reference/template pointer has an explicit owning-directory base. | Inspect all eight canonical Skills and assert every resource-consuming Skill contains the standard local-resource-base instruction. |
| Resource targets use direct `./references/...` or `./templates/...` Markdown links rather than bare code-span paths. | Search all canonical `SKILL.md` files; require zero remaining bare local resource pointers and enumerate every Markdown resource link. |
| Every linked resource resolves from the directory containing its owning `SKILL.md`. | Extract the Markdown targets, join each with its Skill directory, and require `test -e` plus successful `realpath`. |
| Shared contracts remain single-source and all existing symlinks remain valid. | Compare pre/post symlink graph and run `find -L skills -type l -print`, expecting no broken link. |
| Repository-wide copy installation remains readable. | Run a temporary `cp -RL` simulation and repeat local target existence checks in the dereferenced copy. |
| Pointer hardening does not change workflow semantics or conditional loading. | Compare exact non-pointer semantic anchors for routing, authority, planning, TDD, review, governance, and closure; inspect the narrow diff. |
| Maintainer guidance defines the same portable convention without absolute paths or cross-Skill imports. | Inspect `skills/README.md` and search changed files for machine-specific paths or direct `_shared` consumer instructions. |
| The complete change passes Post-execution Engineering and Spec review. | Review the exact approved-plan baseline through all task/fix commits and the explainable worktree; aggregate must be `pass`. |

## Tasks

### Task 1: Characterize current pointer behavior

**Files:** current canonical `skills/**/SKILL.md`, resource paths, symlinks, and
case execution evidence.

- [ ] Record every bare reference/template pointer and its trigger wording.
- [ ] Record the exact reproduced wrong-base path and the correct
  Skill-directory resolution.
- [ ] Capture the current symlink graph, target resolution, copy-install
  behavior, and workflow semantic anchors.

### Task 2: Harden all canonical local resource pointers

**Files:** seven resource-consuming canonical Skills and `skills/README.md`.

- [ ] Add the smallest positive resource-base instruction needed for reliable
  execution.
- [ ] Convert each local resource pointer to a direct relative Markdown link
  while preserving its existing mandatory/conditional trigger.
- [ ] Record the portable authoring convention without changing resource
  ownership or loading policy.
- [ ] Run focused pointer, path, symlink, copy, stale-form, and semantic checks.

### Task 3: Review and close

**Files:** case execution/closure evidence and derived case dashboard.

- [ ] Run final structural, semantic, diff, and Git-scope verification.
- [ ] Perform exact-baseline Engineering and Spec Post-execution review; fix and
  re-review any material finding.
- [ ] Record acceptance, remaining installed-version caveat, commit hashes, and
  the separate push/update/sync acceptance path; close only after the declared
  closure commit succeeds.

## Post-Execution Review

- Engineering review checks link syntax, exact path resolution, symlink/copy
  compatibility, conditional-loading preservation, concise wording, and Git
  scope.
- Spec review checks the recurring wrong-base failure is directly addressed,
  all canonical local resources follow the same convention, workflow behavior
  and single-source ownership remain unchanged, and excluded publication work
  stayed excluded.
- Review baseline is the approved-plan commit; aggregate must pass before
  closure.

## Atomic Commits

1. `docs: approve skill resource pointer plan` with
   `Doc-Loom-Step: plan`.
2. `fix: harden skill resource pointers` with
   `Doc-Loom-Step: task:pointer-hardening` after green verification.
3. Scoped `fix:` commits only for material review findings.
4. `docs: close skill resource pointer case` with
   `Doc-Loom-Step: closure` after final verification.

All commits include
`Doc-Loom-Case: 20260714-skill-resource-pointer-hardening`. Stage only declared
case paths. Push, PR, merge, release, history rewrite, and installed-cache
mutation remain excluded.

## Confirmation Log

| When | Confirmed By | Plan Version | Confirmation |
|---|---|---:|---|
| 2026-07-14T20:05:17+0800 | user | 1 | User replied “执行” after the plan v1 approval summary, authorizing the declared local branch, commits, implementation, verification, review, and closure scope. |
