---
case_id: 20260713-skill-resource-simplification
status: executing
updated_at: 2026-07-13T18:22:13+0800
---

# Execution Report

## Human Summary

- Outcome so far: resource topology and authority/distribution synchronization
  are implemented, committed, and fully green before Post-execution review.
- What changed: canonical resources are 18 -> 12, shared symlinks are 13 -> 8,
  single-owner contracts are local, mandatory context is inlined, generic/dead
  assets are removed, governance rules are co-located, and retained templates
  have exact producer pointers.
- User action needed: none unless execution reaches a declared material stop.
- Local Git effect: branch `codex/skill-resource-simplification`; approved plan
  commit `a219c53`; no push, publication, or history rewrite.

## Plan Reference

- plan_version: 1
- base_commit: `a353e47d366abdcc30cb59cd8d2e9bdc8f102db2`
- adaptive_execution: no; material changes return to plan confirmation.

## Plan Followed

- Current authorization: user said `get things done`, approving plan v1 and
  same-turn execution.
- Run mode: branch.
- Plan commit: `a219c53ebd310d13025a21f0bc5ef647bc977ef7`.
- Scope is limited to the declared Skill resource, authority/distribution,
  case-evidence, and dashboard paths.

## Baseline Characterization

### Resource inventory

- Canonical real resource files: 18.
  - References: 11.
  - Templates: 7.
- Local shared symlinks: 13.
- `_shared` contains four references and one template.
- Common loading costs:
  - default doorway: 1,445 words;
  - context through plan, including the approved exception branch: 3,023 words;
  - typical full persistent flow: 5,203 words;
  - all active unique Skill Markdown: 9,207 words.

### Characterized complexity

| Class | Baseline evidence |
|---|---|
| Single consumer in `_shared` | `loop-protocol.md` has one Skill consumer. |
| Shared file with one complete owner | TDD exception criteria belong to planning; document-update procedure belongs to closure. |
| Mandatory disclosed references | Every context-authority invocation loads both local references; every plan loads risk levels. |
| Weak pointer | Grill loads generic prompts only when it "needs inspiration". |
| Unbound template | Product current-state has no producer pointer; context brief and closure templates lack exact owner pointers; governance template is reached indirectly. |
| Co-located rulebook split | Governance layering/routing and verdict rules are used together by the fixed governance workflow. |

### Behavior locks

| Contract | Required preserved behavior |
|---|---|
| Shared protocol | Remains the one cross-Skill authority/status/artifact/authorization kernel. |
| Context | Same intent, source routing, case/resume, conflict, risk, and verdict behavior after inlining. |
| Risk | Low/medium/high meanings and confirmation outcomes remain unchanged. |
| TDD exception | Only approved plan/amendment; allowed categories and alternative verification remain unchanged. |
| Document update | L1 narrow patch, L2/L3 sync, lifecycle statuses, and proposal boundaries remain unchanged. |
| Governance | Same layers, authority sections, verdicts, block conditions, plan shape, and application gate. |
| Review/grill | Review modes and authorization stay unchanged; grill remains explicit and conversational. |
| Templates | Context brief, plan, execution, closure, governance plan, and handoff keep stable producers and artifact ownership. |
| Distribution | Repository-wide `skillshare` merge/copy installs retain readable required local files. |

## Changes Made

### Shared and local ownership

- Retained only `shared-protocol.md` and `handoff.md` under `_shared`.
- Localized loop discovery to `docloom-workflow`, TDD exception criteria to
  `plan-confirm`, and document-update procedure to `doc-sync-close`.
- Removed the TDD-exception read from execution; execution now validates the
  approved plan's exception contract.
- Expanded the shared risk contract to preserve complete low/medium/high
  classification after removing the always-loaded planning reference.

### Progressive disclosure and templates

- Inlined context intent, source routing, source notes, and run-mode rules into
  `context-authority/SKILL.md`; removed its two mandatory references.
- Removed the generic grill prompt list and the unbound product-state template.
- Merged governance layering, routing verdicts, evidence, entries, bridge,
  archive, and adapter rules into `governance-rules.md`.
- Renamed the governance template to `governance-plan.md` and added direct
  pointers for it, the context brief, and closure templates.
- A user provenance check traced the standalone "Agent Instruction Governance"
  section to repository initialization commit `b5bb4a3`, not Constitution or an
  ADR. Its durable thin-adapter rule is already authority in
  `docs/authority/agent/policy.md`, and its execution steps already belong to
  `setup-doc-governance`; the duplicate standalone reference section was
  removed.

### Final resource topology

- Canonical real resource files: 12.
  - References: 6.
  - Templates: 6.
- Local shared symlinks: 8.
- `_shared` real files: `references/shared-protocol.md` and
  `templates/handoff.md` only.
- Loading costs:
  - default doorway: 1,488 words (43-word named growth from canonical shared
    risk classification; still below 1,700);
  - context through plan: 2,894 words (129 fewer);
  - typical full persistent flow: 5,088 words (115 fewer);
  - all active unique Skill Markdown: 8,599 words (608 fewer).

### Authority and distribution sync

- Updated existing architecture authority with the every-invocation,
  branch/rulebook, exact-template-pointer, and real-local-file boundaries.
- Updated workflow/governance/agent authority source paths to current owners.
- Updated distribution authority and install guidance so `_shared` describes
  only true multi-owner contracts and private local assets are not exhaustively
  listed.
- Updated `skills/README.md` with the corresponding maintainer rules; no new
  authority area, workflow outcome, or installation mode was added.

## TDD Applicability

- TDD Required: No.
- Category: behavior-preserving Skill/document/resource refactor.
- Alternative verification: `characterize -> refactor -> verify` with exact
  resource inventory, pointer/owner graph, semantic anchors, Skill validation,
  symlink/copy checks, `skillshare` audit, word cost, Git delta, and focused
  invocation scenarios. No meaningless Red is claimed.

## Acceptance Criteria Status

| Criterion | Status | Evidence |
|---|---|---|
| Canonical resources <= 12 | met | 18 -> 12 real resource files. |
| Shared symlinks <= 8 | met | 13 -> 8; retained edges expose only shared protocol or handoff. |
| `_shared` only multi-owner contracts | met | Final `_shared` has one reference and one template. |
| Retained references have justified branches/rulebooks | met | Six references map to shared kernel, named branches, or one governance rulebook. |
| Retained templates have exact producer pointers | met | Exact pointer/schema assertions pass for all six templates. |
| Behavior contracts unchanged | met | Final semantic anchors, canonical discovery, pointer resolution, and focused ownership assertions pass. |
| Merge/copy installs readable | met | No broken symlink; copy-mode resource assertions and two `skillshare` audits pass. |
| Loading costs within ceilings | met | Final 1,488 / 2,894 / 5,088 / 8,599. |
| Authority/distribution current | met | Active authority, Skill layout, and install guidance match final paths and ownership. |
| Engineering/Spec review passes | executing | Post-execution gate pending. |

## Review Risk

Medium. Initial review found two material governance-contract issues; fixes are
applied and verification/re-review are in progress.

## Commands Run

| Command | Result | Notes |
|---|---|---|
| Git workspace/branch/status checks | pass | Clean exact baseline before case creation; new branch created after approval. |
| Approved plan YAML and staged diff checks | pass | Plan v1, approval metadata, baseline, scope, and trailers validated. |
| Canonical resource inventory | pass | 18 real references/templates. |
| Shared symlink inventory | pass | 13 readable local symlinks. |
| Unique-path and corpus `wc -w` | pass | 1,445 / 3,023 / 5,203 / 9,207. |
| Exact pointer search | pass | Characterized shared, mandatory, weak, indirect, and unbound resource edges. |
| Eight `quick_validate.py` runs via `uv --with pyyaml` | pass | All canonical Skill frontmatter/body structures valid. |
| Initial template-frontmatter validator | retry | Incorrectly required the intentionally frontmatter-free handoff template; corrected to validate each retained template by its contract. |
| Retained template validation | pass | Four frontmatter templates plus handoff/context-brief schema anchors valid. |
| Resource topology assertion | pass | 12 canonical resources, 8 symlinks, and exactly two `_shared` real files. |
| Exact semantic-anchor assertion | pass | Risk, context verdict/run mode, TDD exception, lifecycle, governance verdict, closure, and template pointers present; removed paths absent. |
| Initial semantic `rg` search | retry | Shell interpreted a backticked search token; replaced by deterministic Ruby string assertions. |
| Copy-style `cp -RL` simulation | pass | Every required local file exists; removed consumer-only files stay absent. |
| `skillshare audit ./skills --format json` | pass | Clean; risk score 0. |
| `skillshare audit ./skills --analyzer cross-skill --format json` | pass | Clean; risk score 0. |
| Final unique-path and corpus `wc -w` | pass | 1,488 / 2,894 / 5,088 / 8,599; all ceilings pass. |
| `git diff --check` | pass | No whitespace errors in the resource delta. |
| Git blame/log provenance for Agent Instruction Governance | pass | Standalone section originated in `b5bb4a3`; current durable rule is already in agent authority and owning Skill procedure. |
| Active stale-path search | pass | Removed shared/local reference and old governance-template paths are absent from authority, Skills, README, and INSTALL. |
| Authority YAML parse and source-path assertions | pass | Five modified authority docs remain active mappings and all cited current Skill paths exist. |
| Authority/distribution semantic assertions | pass | Every-invocation, local ownership, template pointer, shared protocol, and governance-rule claims are present. |
| Exact baseline-to-HEAD Git target | pass | Commits `a219c53`, `6dd5251`, and `35adb8a`; no implementation change remains unstaged. |
| Final eight Skill validators | pass | All canonical Skill structures valid after authority sync. |
| Canonical frontmatter-name assertion | pass | Exactly eight expected invocation names, no duplicate. |
| Final Skill pointer/symlink resolver | pass | Every named local resource exists; no broken symlink. |
| Full-tree copy simulation | pass | Dereferenced copy has no symlink and retains every required resource. |
| Final full and cross-skill audits | pass | Both clean, risk score 0. |
| Review-fix governance gate assertion | pass | The explicit no-active-status current-fact rule is restored in the owning rulebook. |
| Review-fix authority provenance assertion | pass | Agent policy is user-confirmed and cites the approved governance batch. |
| Review-fix validators/audit/cost recheck | pass | Eight Skills valid; cross-skill audit clean; 1,488 / 2,894 / 5,088 / 8,599. |

## Fresh-Agent Scenarios

An independent evidence/complexity review subagent evaluated the changed
contracts without editing repository state:

| Scenario | Result | Evidence |
|---|---|---|
| Context bug, high-risk, and resume routing | pass | Inlined intent/source/run-mode rules plus shared identity/status/resume remain sufficient. |
| Plan with `TDD Required: No` | pass | Planning loads its local exception criteria and records category/reason/verification. |
| Execute an approved TDD exception | pass | Execution consumes and validates the approved plan contract without a criteria-file dependency. |
| Documentation governance routing and plan | pass | Governance owner loads one rulebook and the exact `governance-plan.md` template. |
| Closure authority sync and closure artifact | pass | Closure loads local document-update rules and the exact closure template. |
| Next-slice discovery | pass | Doorway loads the local loop reference only for discovery. |
| Explicit grill | pass | Core question loop produces high-leverage questions without a generic prompt asset. |
| Dereferenced copy install | pass | Required local resources remain readable and removed consumer-only paths remain absent. |

## Post-Execution Review

- Exact baseline: `a353e47d366abdcc30cb59cd8d2e9bdc8f102db2`.
- Review commits: `a219c53`, `6dd5251`, and `35adb8a`.
- Working-tree scope: only this execution-evidence refresh; no staged,
  untracked, unrelated, or pending implementation change.
- Review arrangement: independent Engineering and Spec subagents, plus a
  separate evidence/complexity audit; the main reviewer owns severity,
  deduplication, aggregate, and any fix/re-review loop.

### Initial Engineering

- Verdict: `changes_required`.
- Important F1: the merged governance rulebook retained archive-path status
  rules but dropped the baseline invariant that a document without an active
  status is not current fact.
- Evidence gap: focused fresh-agent scenarios were not yet persisted.

### Initial Spec

- Verdict: `changes_required`.
- Important F1: same governance current-fact admission regression.
- Important F2: `docs/authority/agent/policy.md` declared
  `source_of_truth: adr`, although the thin-adapter rule was promoted through
  the user-approved 2026-07-02 governance batch and ADR-0002 does not contain
  that rule.
- Important F3: behavior preservation was marked met before fresh-agent
  scenario evidence was persisted.
- Minor F4: the active-case dashboard next action was stale.

### Initial Aggregate

- Result: `changes_required`.
- Handling: restore the current-fact status gate, correct agent-policy source
  attribution and source chain, persist the independent scenario evidence,
  refresh the dashboard, verify, commit `review-fix:F1-F2`, then rerun affected
  Engineering and Spec axes.

### Review Fix

- F1: restored the explicit no-active-status current-fact gate in
  `governance-rules.md`.
- F2: changed agent policy to `source_of_truth: user_confirmed` and cited the
  approved governance batch alongside Constitution/ADR/Skill sources.
- F3: persisted eight independent fresh-agent scenarios above.
- F4: refreshed the derived dashboard next action.
- Re-review: pending.

## Commits

| Step | Commit | Status |
|---|---|---|
| plan | `a219c53ebd310d13025a21f0bc5ef647bc977ef7` | complete |
| task:resource-topology | `6dd5251` | complete |
| task:resource-docs | `35adb8a` | complete |
| review-fix:F1-F2 | pending | verification in progress |
| closure | pending | not started |
