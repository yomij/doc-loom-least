---
case_id: 20260713-skill-dependency-boundaries
status: ready_to_close
updated_at: 2026-07-13T17:29:59+0800
---

# Execution Report

## Human Summary

- Outcome so far: dependency-boundary implementation, review fix, complete
  verification, and final Engineering/Spec review pass; execution is ready for
  closure.
- What changed: cross-Skill runtime composition now uses Skill names; TDD and
  document-update contracts moved to `_shared` with local consumer symlinks;
  unused symlinks were removed; handoff ownership and distribution policy are
  explicit.
- User action needed: none unless execution reaches a declared material stop.
- Local Git effect: branch `codex/skill-dependency-boundaries`; approved plan
  `23450b8`, task `74b5d84`, and review fix `66c7c61`; no push or history
  rewrite.

## Plan Reference

- plan_version: 2
- base_commit: `17f783f20cb4a168a2f07ece341b2df272384a7d`
- adaptive_execution: no; material changes return to plan confirmation.

## Plan Followed

- Current authorization: user said `执行` after reviewing plan v2.
- Run mode: branch.
- Plan commit: `23450b88843cc13504d5eb9d0db602a617c5d01f`.
- Scope is limited to the approved Skill dependency, shared reference/symlink,
  architecture/distribution, case-evidence, and dashboard paths.

## Baseline Characterization

### File dependency edges

- Direct cross-Skill file reads:
  - `tdd-execute/SKILL.md` -> `../../assessment/review/SKILL.md`.
  - `setup-doc-governance/SKILL.md` ->
    `../../development/doc-sync-close/references/doc-update-rules.md`.
- Implicit reads without an exact local path:
  - `plan-confirm` says `Read TDD exceptions`.
  - `plan-confirm`, `tdd-execute`, and `doc-sync-close` say to read shared rules
    without naming `references/shared-protocol.md`.
- Runtime Skill-name routes are intentional workflow composition, including
  router-to-stage transitions, `tdd-execute` -> `review`, and return routes to
  planning/closure. They are not physical import edges.

### Symlink inventory and ownership

- Eleven tracked local symlinks expose shared protocol, loop protocol, or
  handoff templates.
- `review/references/shared-protocol.md` has no consumer in `review`.
- `plan-confirm/templates/handoff.md` has no writer/consumer in `plan-confirm`.
- `tdd-execute` and `doc-sync-close` are the current direct `handoff.md`
  producers; `_shared/templates/handoff.md` owns its shape.
- Shared protocol currently labels the concrete handoff artifact owner only as
  `Current stage`; it does not yet separate schema, instance, consumer, and
  status ownership.

### Behavior locks

| Contract | Required preserved behavior |
|---|---|
| Review invocation | Only explicit ad-hoc review or an approved eligible `tdd-execute` Post-execution gate enters `review`. |
| Review ownership | `review` stays read-only; `tdd-execute` persists evidence and owns fix/re-review. |
| TDD exception | Only approved plan/amendment; criteria and alternative verification remain unchanged. |
| Document update | Lifecycle statuses, narrow L1 patch boundary, and L3 mechanical sync remain unchanged. |
| Handoff | Shared template, producer-owned instance, resume-only consumption, never case-status authority. |
| Distribution | All canonical Skills remain discoverable; merge and copy modes retain readable local references. |

### Context-cost baseline

| Surface | Files | Words | Ceiling |
|---|---:|---:|---:|
| Default doorway | shared protocol + `docloom-workflow` | 1,412 | 1,700 |
| Context through plan | 8 unique files | 2,920 | 3,700 |
| Typical full persistent flow | 16 unique files | 5,137 | 7,200 |
| All active unique Skill Markdown | current physical files | 9,079 | 11,100 |

## Changes Made

### Invocation and consumer loading

- `tdd-execute` invokes the installed `review` Skill in `Post-execution` mode
  instead of importing `review/SKILL.md` by filesystem path.
- `plan-confirm`, `tdd-execute`, and `doc-sync-close` now name their local shared
  protocol/reference paths explicitly.
- `tdd-execute` and `doc-sync-close` conditionally load the shared handoff
  template only when writing a real future resume point.

### Shared contracts and symlinks

- Moved TDD exception rules unchanged to
  `skills/_shared/references/tdd-exceptions.md`; both planning and execution
  expose a local `references/tdd-exceptions.md` symlink.
- Moved document update/lifecycle rules unchanged to
  `skills/_shared/references/doc-update-rules.md`; governance and closure expose
  a local `references/doc-update-rules.md` symlink.
- Removed unused `review/references/shared-protocol.md` and
  `plan-confirm/templates/handoff.md` symlinks.
- Verified copy-style dereferencing produces real readable files for every
  retained shared reference/template in all four affected consumer Skills.

### Handoff and durable policy

- Shared protocol now distinguishes shared template ownership, concrete
  producer ownership, resume consumption, and case-status ownership.
- Architecture authority and `skills/README.md` now distinguish runtime
  Skill-name invocation from shared file contracts and private local assets.
- Distribution authority and `INSTALL.md` now describe all current shared
  reference files, distinguish the explicit single-consumer loop reference,
  and retain writer-only handoff plus repository-wide companion-Skill rules.

## TDD Applicability

- TDD Required: No.
- Category: behavior-preserving Skill/document/symlink refactor.
- Alternative verification: characterize current dependency and behavior
  contracts, then verify path/symlink/content equivalence, Skill discovery,
  distribution audit, word cost, frontmatter/Markdown, exact Git delta, and
  fresh-agent scenarios. No meaningless Red is claimed.

## Acceptance Criteria Status

| Criterion | Status | Evidence |
|---|---|---|
| No direct cross-Skill file dependency | met | Cross-path search returns none in active `skills/**`. |
| Canonical Skill-name control flow and authorization preserved | met | `tdd-execute` invokes `review` by name; review's explicit/approved entry gate and read-only boundary remain; scenario assertions pass. |
| Shared TDD/document contracts have one source and local consumer paths | met | Content matches the exact baseline; four local symlinks resolve and copy as real readable files. |
| Retained symlinks have explicit consumers; unused edges removed | met | Two unused symlinks removed; retained shared/handoff paths are named by their consumers. |
| Handoff ownership is unambiguous and status-neutral | met | Shared protocol separates template, producer, consumer, and status ownership; only two direct writers expose the template. |
| File graph acyclic; workflow return routes preserved | met | No direct cross-Skill file edge remains; runtime route names and return gates are unchanged. |
| Supported distribution remains valid | met | Full and cross-skill-only `skillshare audit` pass clean; copy-style dereference and canonical discovery pass. |
| Context-cost ceilings remain satisfied | met | Final 1,445 / 3,023 / 5,203 / 9,207 words; small named growth adds explicit loading and ownership contracts. |
| Authority/distribution accurately describe final model | met | Architecture/distribution frontmatter parses and their text matches the implemented tree. |
| Engineering and Spec review pass | met | Final complete-delta Engineering and affected/full Spec re-review pass; aggregate `pass`. |

## Review Risk

Low. The original medium causes are resolved by exact contract-equivalence,
path/symlink/copy verification, clean `skillshare` audits, bounded cost,
review-fix F1, final aggregate `pass`, and clean pre-evidence worktree scope.

## Deviations

| Plan Boundary | Actual Change | Classification | Handling |
|---|---|---|---|
| Fresh-agent scenarios | Environment policy did not authorize spawning subagents; ran eight isolated deterministic dependency/behavior assertions in the current agent instead. | minor | No implementation scope or acceptance changed; record the limitation for review. |

## Commands Run

| Command | Result | Notes |
|---|---|---|
| `git switch -c codex/skill-dependency-boundaries` | pass | Created approved branch. |
| Plan staged diff/check and semantic commit | pass | Commit `23450b8` has required case/step trailers. |
| Cross-path/implicit-read `rg` plus tracked symlink inventory | pass | Found the two direct paths, three implicit shared reads, and two unused symlinks described above. |
| `skillshare audit ./skills --format json` | pass | One target scanned; clean; risk score 0; no findings. |
| Unique-path and corpus `wc -w` | pass | Baseline stays within all four approved ceilings. |
| `git status`, log, and commit-message inspection | pass | Only expected case evidence is in progress; baseline and plan commit are exact. |
| Eight `quick_validate.py` runs via `uv` | pass | Every canonical Skill frontmatter/body structure is valid. |
| YAML parse via `uv` | pass | Modified authority plus plan/execution frontmatter parse as mappings. |
| Baseline `cmp` for moved contracts | pass | TDD exception and document-update content is unchanged. |
| Broken-symlink and local-read checks | pass | No broken symlink; every new/retained consumer path is readable. |
| Copy-style `cp -RL` simulation | pass | Four affected Skills receive real files for all shared references/templates. |
| `skillshare audit ./skills --analyzer cross-skill --format json` | pass | Cross-skill analyzer reports no findings, risk score 0. |
| Canonical name discovery plus eight scenario assertions | pass | Eight expected Skill names and all dependency/authorization/handoff assertions pass. |
| Final unique-path and corpus `wc -w` | pass | 1,445 / 3,023 / 5,203 / 9,207, all below approved ceilings. |
| `git diff --check` and exact-delta inspection | pass | No whitespace error; changes match approved paths and decisions. |
| F1 shared-reference inventory and YAML checks | pass | Authority/INSTALL list all four shared references; `loop-protocol.md` is identified as the explicit single-consumer reference. |
| F1 cross-skill audit and cost recheck | pass | Audit remains clean; Skill costs remain 1,445 doorway and 9,207 corpus words. |

## Post-Execution Review

- Exact baseline: `17f783f20cb4a168a2f07ece341b2df272384a7d`.
- Reviewed commits: `23450b8` plan and `74b5d84` task.
- Working-tree scope at initial review: only this execution evidence update;
  no staged or untracked file and no unrelated change.

### Initial Engineering

- Verdict: No material issue found.
- Findings: None within reviewed scope.
- Evidence gaps: fresh-agent delegation was unavailable under environment
  policy; deterministic scenario assertions, direct contract inspection,
  validators, audits, and copy simulation provide sufficient evidence for the
  Markdown/symlink implementation surface.
- Scope: complete exact-baseline delta, symlink/file modes, local/copy
  readability, canonical discovery, audit results, cost, and Git isolation.

### Initial Spec

- Verdict: Material issues found.
- Important F1: `docs/authority/operations/distribution.md` and `INSTALL.md`
  said the current shared references were only `shared-protocol.md`,
  `tdd-exceptions.md`, and `doc-update-rules.md`, but the final tree also has
  the active symlinked `loop-protocol.md` reference.
  Evidence: direct `_shared/references/` and symlink inventory contradicts the
  exhaustive wording.
  Impact: active distribution authority and installation guidance do not fully
  describe the implemented shared-reference model, failing the approved Spec
  acceptance criterion.
  Required correction: list `loop-protocol.md` and distinguish it as the
  explicit single-consumer discovery reference.
- Evidence gaps: none material.

### Initial Aggregate

- Result: `changes_required`.
- Handling: correct F1 in the two approved distribution paths, rerun relevant
  structural/distribution/cost checks, commit `review-fix:F1`, and re-review the
  Spec axis plus final aggregate.

### Final Engineering

- Verdict: No material issue found.
- Findings: None within reviewed scope.
- Evidence gaps: no executable workflow interpreter and no authorized fresh
  subagent run; direct semantic assertions, eight Skill validators, YAML,
  content equivalence, symlink/copy checks, two `skillshare` audit modes, exact
  Git evidence, and cost verification are sufficient for this repository's
  Markdown/symlink implementation surface.
- Scope: exact baseline through `66c7c61`, all approved source/authority/
  distribution/case files, commit modes/trailers, and clean worktree before this
  final evidence update.

### Final Spec

- Verdict: No material issue found.
- Findings: None within reviewed scope.
- F1 disposition: resolved. Both distribution authority and `INSTALL.md` list
  all four shared reference files and identify `loop-protocol.md` as the
  explicit single-consumer discovery reference.
- Evidence gaps: none material.
- Scope: plan v2 Goal, Non-goals, Decisions, Files, Acceptance, amendment,
  Constitution, architecture/workflow/distribution authority, supported
  install contract, and the complete accumulated delta.

### Final Aggregate

- Result: `pass`.
- Exact reviewed baseline: `17f783f20cb4a168a2f07ece341b2df272384a7d`.
- Reviewed commits: `23450b8`, `74b5d84`, `66c7c61`.
- Working-tree scope before this final evidence update: no staged, unstaged,
  untracked, or unrelated change.

## Commits

| Step | Commit | Status |
|---|---|---|
| plan | `23450b88843cc13504d5eb9d0db602a617c5d01f` | complete |
| task:dependency-boundaries | `74b5d8454b43f0c941b0229a7e49a7af688ddee8` | complete |
| review-fix:F1 | `66c7c61896206a5cd7dbb378f7250d2c24109fb4` | complete |
| closure | pending | not started |
