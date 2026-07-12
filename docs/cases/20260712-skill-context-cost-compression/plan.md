---
case_id: 20260712-skill-context-cost-compression
plan_version: 1
status: approved
risk_level: high
approved_by: user
approved_at: 2026-07-12T13:03:06+08:00
base_commit: ead85434eb6c01a8801534b92a37271899a8f055
---

# Implementation Plan

## Human Approval Summary

- Outcome: reduce Doc Loom's common Skill loading cost through one compact
  shared kernel, exact trigger boundaries, single semantic ownership, and
  conditional detail loading while preserving observable workflow behavior.
- Material scope: the shared workflow protocol, the six common-path workflow
  and review Skills, their existing references/templates, narrow workflow and
  architecture authority sync, the Skill map, and case evidence.
- Decision needed: approve plan v1 before any Skill or authority contract is
  edited.
- Expected local Git actions / commit count: the planning branch
  `codex/skill-context-cost-compression` now exists. After approval, expect five
  local commits: approved plan, shared/router compression, stage-contract
  compression, cost-policy/derived sync, and closure. Material review findings
  may add narrowly scoped `fix:` commits.
- Likely interruptions: only a material scope/behavior change, a failed cost
  budget requiring a design choice, or an unresolved review finding.
- Excluded actions: no push, PR, merge, tag, release, amend, rebase, squash,
  history rewrite, dependency change, runtime backend, new workflow stage,
  new artifact type, Constitution amendment, or execution/closure artifact
  merge.

## Goal

Compress the semantic and loading footprint of the common Doc Loom workflow,
measured by the unique Markdown words an agent is instructed to read on real
paths rather than raw repository line count alone.

Current clean-main baseline:

| Cost surface | Current words | Required target |
|---|---:|---:|
| Default doorway: `docloom-workflow` + shared protocol | 3,112 | <= 1,700 |
| Context through plan, including current conditional references/template | 6,085 | <= 3,700 |
| Typical full persistent flow through review and closure | 10,851 | <= 7,200 |
| All active `skills/**` Markdown | 14,692 | <= 11,100 |

Words are a stable repository proxy for context cost, not a claim about exact
runtime tokens. Final evidence must also report loaded file count and route
composition so savings cannot be created by moving prose to another mandatory
file.

## Non-goals

- Do not change routing outcomes, stage ownership, approval semantics, risk
  meaning, TDD defaults/exceptions, Post-execution review, atomic commits,
  closure gates, authority ordering, or Git authorization.
- Do not remove or rename Skills, artifacts, closure statuses, risk levels, or
  supported workflow modes.
- Do not merge `execution.md` and `closure.md`; the newly shipped
  artifact-owned status contract must be dogfooded independently first.
- Do not compress discovery, grill, complexity-only review, or governance-only
  paths unless a common-path edit makes a small alignment change unavoidable.
- Do not add a budget checker, CLI, dependency, generic protocol shard, or
  auxiliary documentation artifact in this case.
- Do not rewrite historical cases.

## Context

- Intent: workflow / Skill design; behavior-preserving semantic refactor.
- Route Verdict: `proceed_to_plan`.
- Proposed and selected case: `20260712-skill-context-cost-compression`.
- User decision so far: continue from the proposed cost-focused compression
  direction; formal high-risk execution confirmation remains pending on this
  complete plan and its Git effects.
- Active cases before creation: none. The preceding artifact-owned status case
  is committed, closed, and present on clean `main`.
- Authority fit: the Constitution requires the minimum path, human-semantic
  design, fewer moving parts, and no process added for appearance. Skill
  Creator guidance treats context as a shared resource and recommends exact
  trigger metadata, lean Skill bodies, and conditional references.

Context sources:

| Source | Why included | Trust |
|---|---|---|
| `docs/authority/constitution.md` | Minimum-path and human-semantic constraints. | Highest |
| `docs/authority/README.md` | Current fact order and workflow policy entry. | High |
| `docs/authority/workflow/development-flow.md` | Current stages, confirmation, review, and Done contract. | High |
| `docs/authority/agent/policy.md` | Context gate and workflow-policy risk classification. | High |
| `docs/authority/architecture/repo-and-skills.md` | Current Skill groups and public-entry contract. | High |
| Current `skills/**` | Observable Skill triggers, required reads, owners, and templates. | High |
| `20260710-compress-workflow-skill-text` | Shows the previous file-level pass and retained behavior. | Historical evidence |
| `20260712-artifact-owned-case-state` | Establishes the new artifact-owned status baseline. | Recent implementation evidence |
| Current-turn user request | Defines cost control as the primary objective. | Pending this plan's approval |

Excluded context:

- Archived design material: current authority and Skill implementation are
  sufficient and no historical conflict is present.
- Runtime code/tests: the repository has no workflow interpreter or runtime
  source tree.
- Installed/cached copies outside this repository: distribution validation may
  inspect them later, but repository authority and current source own the plan.

## Decisions

1. Optimize three progressive-disclosure layers separately: always-visible
   frontmatter descriptions, triggered `SKILL.md` bodies, and conditional
   references/templates.
2. Keep one shared protocol file, but reduce it to the cross-skill kernel only:
   ownership, instruction/fact authority, artifact/status invariants,
   confirmation authorization, final statuses, and minimal Git compatibility.
3. Put complete procedural detail at exactly one owning Skill/reference.
   Consumers keep only their trigger, required input, local action, result, and
   safety gate.
4. `docloom-workflow` remains the only broad human-facing trigger. Stage Skills
   state their internal/explicit route precisely; `review` remains explicit or
   workflow-owned, and `grill` remains explicit only.
5. Existing references are conditional by named situation. No new reference is
   allowed in v1; discovering a real need is a material plan amendment.
6. Templates carry artifact shape only when the artifact is being written;
   Skill bodies do not restate template fields and commentary.
7. The common path is the primary hard budget. Total corpus reduction is also
   required, but rare discovery/governance/assessment variants are not loaded
   merely to meet a global density target.
8. Cost policy is recorded as a short architecture/workflow principle and a
   compact maintainer-facing map, not a new workflow phase or artifact.
9. Forward tests use fresh agents with task-like prompts and no expected-answer
   leakage. They are read-only validation, not delegated implementation.

## Workspace Baseline

- Base Commit: `ead85434eb6c01a8801534b92a37271899a8f055`
- Branch Before Planning: `main`
- Planned Run Mode: `branch`
- Planned Branch: `codex/skill-context-cost-compression`
- Dirty Workspace Before Case: no
- Existing Changed Files Before Case: none
- Active Skill Markdown: 27 files / 2,727 lines / 14,692 words

## Risk Level

High. The desired result is reversible Markdown and should not change behavior,
but the edited text is the active workflow contract. Removing or relocating one
authorization, review, resume, Git, or closure rule could weaken an L1-critical
fact or make a route unreliable.

Risk is controlled through pre-edit semantic characterization, per-route cost
accounting, explicit ownership, independent forward tests, complete-delta
Engineering/Spec review, and atomic review fixes.

## TDD Applicability

- TDD Required: No.
- If No, Reason: pure documentation/Skill behavior-preserving refactor with no
  executable workflow interpreter.
- Alternative Verification:
  - characterize current trigger, route, ownership, authorization, review,
    resume, closure, and degraded-Git behavior before editing;
  - measure unique word/file cost for each accepted route before and after;
  - validate every modified Skill and parse selected YAML/frontmatter;
  - run semantic positive and stale-rule assertions, link/symlink checks, and
    `git diff --check`;
  - run fresh-agent prompt tests and mandatory Engineering/Spec review.

## Files to Change

Common shared and entry path:

- `skills/_shared/references/shared-protocol.md`
- `skills/development/docloom-workflow/SKILL.md`

Context and planning path:

- `skills/development/context-authority/SKILL.md`
- `skills/development/context-authority/references/context-resolution.md`
- `skills/development/context-authority/references/retrieval-routing.md`
- `skills/development/plan-confirm/SKILL.md`
- `skills/development/plan-confirm/references/risk-levels.md`
- `skills/development/plan-confirm/templates/plan.md`

Execution, review, and closure path:

- `skills/development/tdd-execute/SKILL.md`
- `skills/development/tdd-execute/references/tdd-exceptions.md`
- `skills/development/tdd-execute/templates/execution.md`
- `skills/assessment/review/SKILL.md`
- `skills/development/doc-sync-close/SKILL.md`
- `skills/development/doc-sync-close/references/doc-update-rules.md`
- `skills/development/doc-sync-close/templates/closure.md`
- `skills/_shared/templates/handoff.md`

Authority and derived map:

- `docs/authority/architecture/repo-and-skills.md`
- `docs/authority/workflow/development-flow.md`
- `skills/README.md`
- `docs/cases/README.md`

Case evidence:

- `docs/cases/20260712-skill-context-cost-compression/plan.md`
- create `docs/cases/20260712-skill-context-cost-compression/execution.md`
- create `docs/cases/20260712-skill-context-cost-compression/closure.md`

## Acceptance Criteria

| Criteria | Planned verification |
|---|---|
| Default doorway path is at most 1,700 words. | Unique-path `wc -w` manifest and file list. |
| Context-through-plan path is at most 3,700 words. | Route composition and unique-path totals. |
| Typical full persistent path is at most 7,200 words. | Route composition through review and closure. |
| All active Skill Markdown is at most 11,100 words, with no target file growing without an explicit semantic reason. | Full-tree counts and per-file before/after table. |
| Savings are not produced by moving prose to another mandatory file. | Before/after required-read graph and changed-reference inspection. |
| Frontmatter descriptions preserve positive and negative trigger boundaries while reducing ambiguity and accidental multi-triggering. | YAML parse, exact description comparison, positive/negative prompt matrix. |
| Every material workflow rule has one complete owner; consumers retain sufficient local triggers/actions/gates. | Semantic ownership matrix and Spec review. |
| Status-only, planning, Fast-Path, full execution, resume, degraded Git, Post-execution review, closure, and narrow authority-patch behavior remain intact. | Characterization assertions plus fresh-agent scenarios. |
| Current artifact-owned status and resumable-closure semantics remain exact. | Positive/stale semantic searches and direct review. |
| No new Skill, reference, stage, artifact type, risk level, dependency, runtime component, or Constitution change is introduced. | Tree/diff scope inspection. |
| Modified Skills, frontmatter, templates, local links, and symlinks remain valid. | `quick_validate.py`, YAML parse, link/symlink checks, `git diff --check`. |
| Independent Engineering and Spec review aggregate passes with no unresolved material finding or evidence gap. | Final review evidence in `execution.md` and closure summary. |

## Tasks

### Task 1: Approve and characterize the cost contract

**Files:** plan and case dashboard.

- [x] Confirm plan v1 and record approval metadata.
- [x] Capture per-file counts, trigger descriptions, required-read edges, and
  unique path totals at the exact baseline.
- [x] Build the semantic ownership and scenario matrix in `execution.md`.
- [x] Commit the approved plan as the declared plan unit.

### Task 2: Compress the shared kernel and human doorway

**Files:** shared protocol, `docloom-workflow`, and execution evidence.

- [x] Retain only cross-skill invariants in the shared kernel.
- [x] Reduce the doorway to start checks, mode selection, case identity,
  routing, output, and hard gates.
- [x] Preserve artifact-owned status, confirmation scope, Fast-Path, resume,
  degraded Git, and no-backend boundaries at their approved owners.
- [x] Verify the 1,700-word doorway budget and commit the coherent green unit.

### Task 3: Compress stage contracts and conditional reads

**Files:** context, plan, execute, review, close, selected existing references,
templates, and execution evidence.

- [x] Rewrite each Skill around trigger, inputs, ordered actions, owned output,
  and gates; delete generic model guidance and consumer restatement.
- [x] Make every reference/template read conditional and one level deep.
- [x] Tighten frontmatter descriptions without dropping trigger coverage.
- [x] Verify the planning/full-flow budgets and commit the coherent green unit.

### Task 4: Record the durable cost principle and validate the full surface

**Files:** architecture/workflow authority, `skills/README.md`, dashboard, and
execution evidence.

- [x] Add only the narrow progressive-disclosure/single-owner principle needed
  to prevent immediate text-cost regression.
- [x] Record the current cost map and ownership route compactly in the Skill
  map; do not duplicate stage procedure.
- [x] Run complete structural, semantic, cost, and changed-file checks.
- [x] Commit the authority/derived sync after it independently passes.

### Task 5: Forward-test, review, fix, and close

- [ ] Run 8-12 fresh-agent task prompts covering ordinary, edge, and negative
  trigger cases without leaking expected answers.
- [ ] Run isolated Engineering and Spec review against the complete delta.
- [ ] Fix each material root cause in the smallest atomic commit and re-review
  the affected axes.
- [ ] Map final acceptance, write closure, update dashboard, create and verify
  the closure commit, and report `Done` only when case scope is clean.

## Post-Execution Review Strategy

- Exact baseline: `ead85434eb6c01a8801534b92a37271899a8f055`.
- Engineering axis: verify trigger reliability, required-read graph accuracy,
  cost calculations, reference reachability, structural validity, artifact
  status/resume safety, Git scope, and absence of hidden new machinery.
- Spec axis: compare the final delta with all Goals, Non-goals, Decisions,
  Acceptance Criteria, Constitution, active workflow/agent authority, the
  previous compression case, and the artifact-owned status contract.
- The axes run independently. Critical/Important findings produce the smallest
  coherent `fix:` commit and affected-axis re-review; missing material evidence
  cannot pass.
- Durable evidence lives in `execution.md` and `closure.md`; create no
  `review.md` or cost-report artifact.

## Atomic Commit Strategy

Expected local commits after approval:

| Step | Commit title | Trailer |
|---|---|---|
| Approved plan | `docs: approve skill context cost compression plan` | `Doc-Loom-Step: plan` |
| Shared/router | `refactor: compress shared workflow loading path` | `Doc-Loom-Step: refactor:shared-loading` |
| Stage contracts | `refactor: compress stage skill contracts` | `Doc-Loom-Step: refactor:stage-contracts` |
| Cost policy and maps | `docs: record skill loading cost policy` | `Doc-Loom-Step: task:cost-policy` |
| Review fix, only if needed | `fix: <root cause>` | `Doc-Loom-Step: review-fix:<id>` |
| Closure | `docs: close skill context cost compression case` | `Doc-Loom-Step: closure` |

All commits use
`Doc-Loom-Case: 20260712-skill-context-cost-compression`, explicit path
staging, staged-diff inspection, `git diff --cached --check`, and relevant
passing verification. Approval authorizes only these case-scoped local commits
and excludes publication, history rewriting, unrelated files, and material
scope changes.

## Risks

| Risk | Mitigation |
|---|---|
| A deleted sentence silently removes a safety gate. | Pre-edit scenario/ownership characterization and final Spec review. |
| Progressive disclosure hides a rule from the consumer that needs it. | Required-read graph plus fresh-agent edge scenarios. |
| Trigger descriptions become shorter but less accurate. | Positive/negative prompt matrix and explicit public/internal trigger roles. |
| Cost target encourages dense, unreadable prose. | Human-semantic review; budgets are ceilings, not density quotas. |
| Savings merely move text to references or authority. | Unique required-path totals, corpus total, and changed-read-edge inspection. |
| The new cost policy becomes extra ceremony. | No new artifact, checker, stage, or script; only concise authority/map text. |

## Documentation Impact

Confirmed only on approval of this plan:

- `docs/authority/architecture/repo-and-skills.md`: add the narrow rule that
  Skill design uses exact triggers, lean bodies, conditional references, and a
  single semantic owner to control context cost.
- `docs/authority/workflow/development-flow.md`: align its shared-kernel pointer
  and retain the same workflow outcomes without restating compressed procedure.
- `skills/README.md`: add a compact maintainer-facing loading/ownership map and
  current budget, without becoming an authority source.

No Constitution, agent-policy, Git-policy, governance-structure, or historical
case change is planned.

## Confirmation Log

| When | Confirmed By | Plan Version | Confirmation |
|---|---|---:|---|
| 2026-07-12T13:03:06+08:00 | user | 1 | `批准` |
