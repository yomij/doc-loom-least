---
title: V1 Clean Execution Plan
type: implementation-plan
status: proposed
created: 2026-06-23
scope:
  - skills
  - shared-protocol
  - references
  - templates
source_plans:
  - docs/design/v1-clean/v1-cleanup-plan.md
  - docs/design/v1-clean/skills-simplification-plan.md
  - docs/design/v1-clean/skills-review-refactor-plan.md
  - docs/design/v1-clean/skills-cleanup-independent-plan.md
  - docs/design/v1-clean/skills-text-cleanup-plan.md
authority:
  - docs/adr/ADR-0000-constitution.md
  - docs/design/document-driven-workflow-skills-final.md
---

# V1 Clean Execution Plan

> **For agentic workers:** REQUIRED SUB-SKILL: use `superpowers:subagent-driven-development` or `superpowers:executing-plans` to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Reduce redundant prose in Doc Loom Least skills without changing workflow behavior.

**Architecture:** Keep `SKILL.md` files as stage-owned operating instructions and keep `_shared/references/shared-protocol.md` as the cross-skill vocabulary and constraint owner. Prefer deletion, consolidation, and references over new stages, new artifacts, or a protocol split.

**Tech Stack:** Markdown skill files, symlinked shared references, shell verification with `rtk`, no package-manager changes.

---

## Current Conclusion

The five existing plans agree on the core cleanup direction: trim descriptions,
remove duplicate openers, consolidate shared protocol duplication, deduplicate
high-risk wording, tighten Gates, and consolidate templates where semantics are
not unique.

Several proposed changes need correction before execution:

- Do not merge `Execution Instruction Order` and `Fact Authority Order`.
  `docs/design/document-driven-workflow-skills-final.md` explicitly split them
  to prevent temporary user instructions from becoming long-term facts.
- Do not remove local safety boundaries from `review` and `grill` merely because
  shared protocol has similar constraints. They are manual helpers and must be
  safe when invoked directly.
- Do not delete `tdd-execute` Gates about authority docs, dependencies,
  lockfiles, CI, or non-goals unless the same stop condition is preserved at the
  decision point.
- Do not delete `doc-sync-close/templates/case_state.yaml` unless the close-state
  patch shape is moved into `doc-sync-close/SKILL.md` or shared protocol.
- Handoff templates may be consolidated only if the shared template preserves
  `Closure Status` and `Resume Condition` slots used by closure handoff.

## File Responsibility After Cleanup

| File area | Responsibility |
|---|---|
| `skills/*/SKILL.md` | Trigger boundary, stage workflow, stage-owned Gates, output contract |
| `skills/_shared/references/shared-protocol.md` | Cross-skill ownership, priority orders, case state, artifact policy, confirmation semantics, high-risk vocabulary |
| `skills/development/context-authority/references/*` | Context intent, retrieval routing, source filtering |
| `skills/development/doc-sync-close/references/doc-update-rules.md` | Closure-time document update and authority patch rules |
| `skills/governance/setup-doc-governance/references/*` | Governance layering, verdicts, authority routing |
| `skills/*/templates/*` | Output skeletons only, not policy |

## File-by-File Rationale

### `skills/_shared/references/shared-protocol.md`

Execute:

- Add `## High-Risk Topics` after `Fact Authority Order`.
  Reason: high-risk domains are repeated across context, review, grill,
  execution, governance, and doc-update rules. A canonical term prevents drift.
  Keep the list semantic, not a second risk policy.
- Merge `case_state.yaml` conflict summary with `Case State Conflicts`.
  Reason: current lines define the same conflict rule twice; the detailed
  derivation list is the useful version.
- Delete `Session State Carry` as a separate heading after preserving its useful
  sentence in `case_state.yaml`.
  Reason: the carry contract already belongs to routing fields.
- Merge the standalone `closure.md` row into the Artifact Policy table.
  Reason: a split table makes closure look like an exception when it is an
  artifact policy row.
- Compress `Skill Runtime Boundary`.
  Reason: all five bullets express two constraints: no backend/control files and
  no replacing stage skills.
- Compress `Design Guardrails` but keep the target-project source-of-truth
  sentence.
  Reason: constitution owns the philosophy; this file still needs installation
  boundary guidance.

Do not execute:

- Do not merge `Execution Instruction Order` and `Fact Authority Order`.
  Reason: the final design document records this split as intentional behavior,
  not redundant prose.
- Do not split shared protocol in v1 clean.
  Reason: text-only cleanup should be verified before adding reference topology.

### `skills/development/docloom-workflow/SKILL.md`

Execute:

- Trim frontmatter description to trigger metadata.
  Reason: the current description is a feature list; routing details belong in
  the body.
- Keep the Start command block here.
  Reason: `docloom-workflow` is the public entry and status resolver; it owns the
  initial workspace check.
- Merge Route Table rows 6 and 8.
  Reason: both route to `context-authority`; the distinction is not actionable.
- Keep fast-path details by reference to shared protocol.
  Reason: this skill chooses the route, shared protocol owns the full fast-path
  contract.
- Tighten Gates by removing Route Table restatements.
  Reason: Gates should stop unsafe progression, not repeat every route row.

Preserve:

- Case identity ownership and delayed case creation.
- Status-only no-write behavior.
- No automatic execution, review, or grill.
- No `base_commit` or `risk_level` writes from this skill.

### `skills/development/context-authority/SKILL.md`

Execute:

- Trim description and remove the duplicate opener while preserving read-only
  boundaries.
- Change Start to reuse `docloom-workflow` checks when available; otherwise run
  the same minimal checks.
  Reason: this skill can be invoked directly, so it still needs a fallback.
- Replace inline public API/security/etc. conflict list with
  `shared-protocol.md -> High-Risk Topics`.
  Reason: the list is cross-skill vocabulary.
- Tighten Gates to true stops: no context, blocking conflict, and stale or
  low-authority evidence.

Preserve:

- It must not create case identity.
- It must not write implementation plans or modify artifacts.
- It must output a route verdict, not a plan.

### `skills/development/plan-confirm/SKILL.md`

Execute:

- Trim description and remove the duplicate opener.
- Keep the case identity boundary.
- Replace `Version And Confirmation` prose with a state-transition table.
  Reason: the current section describes three state changes in prose; a table is
  shorter and less ambiguous.
- Delete `Plan Quality Standard` only after moving its unique rule,
  `No placeholders`, into `Plan Rules` or the self-review step.
  Reason: exact paths and commands are already required, but the placeholder ban
  must survive.
- Remove Gates that only repeat workflow phase-setting.

Preserve:

- No plan without context or explicit skipped-context reason.
- No plan without case identity.
- No execution without user confirmation of the current `plan_version`.
- Material plan changes reset approval.

### `skills/development/tdd-execute/SKILL.md`

Execute:

- Trim description and remove the duplicate opener.
- Compress testing anti-pattern bullets into two compact sentences while keeping
  all five ideas.
- Merge `Review Risk` and `Review Risk Lifecycle`.
  Reason: one section should define values, write/update conditions, and the
  rule that review is not auto-triggered.
- Replace repeated high-risk list in review-risk logic with `High-Risk Topics`,
  but keep local execution-specific examples such as coverage gaps, authority
  proposal, and serious plan deviation.
- Shorten the `State Update` opening sentence; keep the YAML state examples.

Preserve:

- Approved plan and current `plan_version` preflight.
- Observed expected Red before implementation when TDD applies.
- TDD exception requires alternative verification.
- Serious deviation routes back to `plan-confirm`.
- Non-goals, authority docs, dependencies, lockfiles, CI, scripts, and test
  command config remain protected unless authorized.

### `skills/development/doc-sync-close/SKILL.md`

Execute:

- Trim description and remove the duplicate opener.
- Replace local case-identity ban with a shared-protocol reference while keeping
  the route-back behavior.
- Compress Closure Status guidance to the two local distinctions:
  `Done` and `Done with Caveats`; other meanings can point to shared protocol.
- Remove examples from Knowledge Changes but keep the promotion rule.
- Keep closure-before-state-update once in the most decision-relevant location.
- Convert self-routes in Gates to `wait for user input`.

Preserve:

- Authority changes require explicit concrete confirmation.
- Unmet acceptance cannot be unqualified `Done`.
- Missing required `execution.md` cannot be unqualified `Done`.
- Closure consumes `review_risk`.
- Closure must not modify code/tests/dependencies/scripts/lockfiles/CI or
  acceptance criteria to make closure pass.

### `skills/assessment/review/SKILL.md`

Execute:

- Trim description.
- Keep the opening manual-trigger and read-only boundary.
  Reason: this is the primary safety rule for direct invocation.
- Remove the extra historical/derived/scratch sentence under `Fact Authority`
  after referencing shared protocol.
- Replace the high-risk topic list in Review Checks with a canonical reference,
  but keep the review-specific rule: missing direct evidence is a finding or gap.
- Tighten Gates to two true stops: explicit user request and missing evidence
  cannot be packaged as passing.

Preserve:

- No files, state, artifacts, routes, closure verdicts, or authority proposals.
- Smallest relevant evidence selection.
- Findings must be locatable and verifiable.

### `skills/assessment/grill/SKILL.md`

Execute with adjustment:

- Trim description.
- Keep local no-file/no-artifact boundary.
  Reason: `grill` currently has no `references/shared-protocol.md` symlink and is
  intentionally standalone.
- Either keep a one-line local high-risk list with canonical wording, or add a
  `references/shared-protocol.md` symlink before replacing the list with a
  reference.
  Preferred v1 clean choice: keep one local canonical line to avoid adding a new
  dependency for a conversation-only helper.
- Compress high-risk recommendation bullets, but keep lower certainty,
  evidence-source disclosure, and no unverified facts.
- Tighten Gates to explicit grill request, one-question-at-a-time, verify facts
  when answerable, no file/artifact changes, and no unconfirmed decisions.

### `skills/governance/setup-doc-governance/SKILL.md`

Execute:

- Trim description.
- Keep the opener because it distinguishes historical evidence from authority.
- Replace the negative legacy artifact list with an Artifact Policy reference.
  Reason: policy should say what may be created, not enumerate every old file not
  to create.
- Replace local authority status values with
  `doc-update-rules.md -> Lifecycle Status`.
  Reason: status values already have a single owner.
- Move `Update the docs entry index after applying decisions` out of Gates and
  into Workflow after apply.
- Remove Gates that merely repeat Authority Rules, but keep the underlying rules
  in Authority Rules.

Preserve:

- Default scope is `docs-only`.
- `full-repo` is read-only evidence only.
- No application before user confirmation.
- No new governance batch into an existing plan file.
- Bridges must not carry old facts.

### `skills/development/context-authority/references/context-resolution.md`

Execute:

- Remove the duplicated resume-from-closed paragraph.
- Replace the `Default exclusions` table column with one sentence.
  Reason: the column is mostly illustrative and increases scan cost.
- Move or duplicate `Run Modes` into shared protocol only if `plan-confirm` needs
  to reason about run mode without loading this reference.
  Preferred v1 clean choice: move `Run Modes` to shared protocol because
  `plan-confirm` already reads shared protocol.

Preserve:

- Intent classification.
- Context-authority does not create branches, worktrees, or case docs.

### `skills/development/context-authority/references/retrieval-routing.md`

Execute:

- Convert Governance Docs rules into a short priority list.
- Replace high-risk inline wording with `High-Risk Topics`.
- Replace case-state conflict prose with shared-protocol reference.

Preserve:

- Missing authority can proceed with risk for low-risk local work.
- High-risk/public-contract/workflow/agent-policy work routes to governance or
  user decision when governance is missing.
- Every included source needs `why_included`.

### `skills/development/doc-sync-close/references/doc-update-rules.md`

Execute:

- In Narrow Authority Patch, replace the high-risk conflict condition with a
  reference to `High-Risk Topics`.
- Keep `Lifecycle Status` as the owner of authority status values.

Preserve:

- Narrow patch requires existing authority doc, small existing fact, evidence,
  no new authority area, no movement/archive/bridge, no lifecycle migration, and
  concrete user confirmation.

### `skills/governance/setup-doc-governance/references/verdicts.md`

Execute:

- Replace high-risk block wording with canonical wording or a reference to
  `High-Risk Topics`.
- Replace the Plan Tables code blocks with a pointer to
  `templates/GOVERNANCE_PLAN.md`.
  Reason: table skeletons are template responsibility.

Preserve:

- Verdict set: `promote`, `merge`, `bridge`, `archive`, `block`.
- Code/tests are read-only evidence.
- Code/tests can override historical docs only for low-risk internal details
  when active authority is not involved and tests agree.

### `skills/governance/setup-doc-governance/references/layering-and-routing.md`

Execute:

- No major cleanup required.
- Use this file as the owner for index update rules after moving the operational
  instruction out of `setup-doc-governance` Gates.

### Handoff templates

Execute with adjustment:

- Create `skills/_shared/templates/handoff.md` only if the shared template keeps
  all slots currently used by stage templates:
  `Current Phase`, `Closure Status`, `Last Completed`, `Resume Condition`,
  `Next Step`, `Do Not Touch`, `Commands Last Run`, `Known Issues`, and
  `Source of Detailed History`.
- Update `plan-confirm`, `tdd-execute`, and `doc-sync-close` references to the
  shared template.
- Delete per-skill handoff templates only after references are updated.

Reason: the existing templates are mostly the same skeleton, but
`doc-sync-close` has closure-specific resume semantics that must not be lost.

### Case state templates

Do not execute the deletion by default:

- Keep `skills/development/docloom-workflow/templates/case_state.yaml` as the initial case
  state template.
- Keep `skills/development/doc-sync-close/templates/case_state.yaml` unless its four-field
  close-state shape is moved into `doc-sync-close/SKILL.md` or shared protocol.

Reason: these templates serve different moments: initial case creation versus
closure patch shape.

## Execution Tasks

### Task 0: Baseline

- [ ] Run:

  ```bash
  rtk wc -l skills/*/SKILL.md skills/_shared/references/shared-protocol.md skills/*/references/*.md skills/*/templates/* 2>/dev/null
  rtk rg -n "security|auth|permission|privacy|billing|data deletion|public API|CLI|schema|config contract|ADR-protected|migration" skills
  rtk rg -n "Route:|Required input|## Gates|Session State Carry|Case State Conflicts|Review Risk|Plan Quality Standard" skills
  rtk git status --short -- skills docs/design/v1-clean
  ```

- [ ] Record the counts and notable matches in the implementation notes, not a
  new artifact.

### Task 1: Shared protocol cleanup

- [ ] Edit `skills/_shared/references/shared-protocol.md`.
- [ ] Add `High-Risk Topics`.
- [ ] Merge case-state conflict sections.
- [ ] Merge Session State Carry into `case_state.yaml`.
- [ ] Merge `closure.md` into the Artifact Policy table.
- [ ] Compress Runtime Boundary and Design Guardrails.
- [ ] Verify:

  ```bash
  rtk rg -n "High-Risk Topics|Session State Carry|Case State Conflicts|closure.md|Execution Instruction Order|Fact Authority Order" skills/_shared/references/shared-protocol.md
  ```

Expected: High-Risk Topics appears once; Session State Carry and Case State
Conflicts headings are gone; both priority-order headings still exist.

### Task 2: SKILL descriptions and duplicate openers

- [ ] Edit all eight `skills/*/SKILL.md` files.
- [ ] Replace descriptions with trigger-oriented descriptions.
- [ ] Remove opener sentences only when they duplicate the description.
- [ ] Preserve safety boundaries for `review`, `grill`, and stage ownership.
- [ ] Verify:

  ```bash
  rtk rg -n "^description:" skills/*/SKILL.md
  rtk sed -n '1,25p' skills/development/docloom-workflow/SKILL.md
  rtk sed -n '1,25p' skills/development/context-authority/SKILL.md
  rtk sed -n '1,25p' skills/development/plan-confirm/SKILL.md
  rtk sed -n '1,25p' skills/development/tdd-execute/SKILL.md
  rtk sed -n '1,25p' skills/development/doc-sync-close/SKILL.md
  ```

Expected: descriptions are concise; bodies begin with operating guidance, not
description echoes.

### Task 3: Cross-file high-risk and shared references

- [ ] Update `context-authority`, `review`, `tdd-execute`,
  `retrieval-routing.md`, `doc-update-rules.md`, and `verdicts.md` to reference
  `High-Risk Topics`.
- [ ] Keep `grill` local canonical wording unless a shared-protocol symlink is
  intentionally added for it.
- [ ] Verify:

  ```bash
  rtk rg -n "security, auth|public API, CLI|ADR-protected|schema migration|data deletion" skills
  ```

Expected: only the shared canonical list and explicitly justified local copies
remain.

### Task 4: Gates cleanup

- [ ] Tighten Gates in all eight `SKILL.md` files.
- [ ] Replace self-routes that mean "ask/stop" with `wait for user input`.
- [ ] Keep local direct-invocation safety for `review` and `grill`.
- [ ] Keep execution-critical stop conditions in `tdd-execute`.
- [ ] Verify:

  ```bash
  rtk rg -n "## Gates|Route:|Required input|wait for user input" skills/*/SKILL.md
  ```

Expected: route syntax is used for real cross-skill handoff; Gates no longer
repeat body rules without adding a stop condition.

### Task 5: Local skill cleanup

- [ ] Merge `docloom-workflow` Route Table rows 6 and 8.
- [ ] Table-ify `plan-confirm` Version And Confirmation and preserve
  `No placeholders`.
- [ ] Merge `tdd-execute` review-risk sections.
- [ ] Trim `doc-sync-close` Knowledge Changes examples.
- [ ] Replace `setup-doc-governance` legacy artifact ban with Artifact Policy
  reference and move index update into Workflow.
- [ ] Verify:

  ```bash
  rtk rg -n "Plan Quality Standard|Review Risk Lifecycle|FACT_REGISTRY|KNOWLEDGE_CARDS|CONFLICT_REPORT|AUTHORITY_REBUILD_PLAN|CONTEXT_PACK_POLICY|Route Table" skills
  ```

Expected: old redundant headings or legacy artifact names are gone unless a
short compatibility warning was intentionally retained.

### Task 6: References cleanup

- [ ] Remove duplicate resume guidance from `context-resolution.md`.
- [ ] Convert retrieval governance rules to a priority list.
- [ ] Point authority status references to `doc-update-rules.md -> Lifecycle Status`.
- [ ] Replace verdict table skeletons with a pointer to `GOVERNANCE_PLAN.md`.
- [ ] Verify:

  ```bash
  rtk rg -n "Resume-from-closed|Allowed `status`|Lifecycle Status|Plan Tables|GOVERNANCE_PLAN.md" skills
  ```

Expected: lifecycle status has one owner; plan table skeletons are template-owned.

### Task 7: Template cleanup

- [ ] Create `skills/_shared/templates/handoff.md` only with all required slots.
- [ ] Update SKILL references to the shared handoff template.
- [ ] Delete redundant handoff templates after reference updates.
- [ ] Keep or relocate `doc-sync-close/templates/case_state.yaml` intentionally.
- [ ] Verify:

  ```bash
  rtk rg -n "templates/handoff.md|_shared/templates/handoff.md|doc-sync-close/templates/case_state.yaml" skills
  rtk test -f skills/_shared/templates/handoff.md
  ```

Expected: no deleted template is still referenced; closure resume semantics remain
available.

### Task 8: Final verification

- [ ] Run:

  ```bash
  rtk rg --files skills
  rtk rg -n "T[B]D|T[O]DO|f[i]ll in|implement late[r]" skills
  rtk rg -n "description:" skills/*/SKILL.md
  rtk rg -n "status: needs_refresh|status: in_review" skills
  rtk git diff -- skills
  ```

- [ ] Manually verify:
  - Every behavior rule still exists in at least one decision-relevant location.
  - `review` and `grill` remain explicit user-only helpers.
  - `docloom-workflow` remains a thin router and case identity owner.
  - `plan-confirm` remains the only plan confirmation gate.
  - `tdd-execute` still requires approved plan and expected Red when TDD applies.
  - `doc-sync-close` cannot mark unmet acceptance as unqualified `Done`.
  - Authority changes still require explicit confirmation unless they are
    confirmed narrow patches.
  - Diff shows prose consolidation and reference updates only.

## Acceptance Criteria

- Net skill text is at least 20% shorter, excluding symlink duplicates.
- No `SKILL.md` description is a feature list.
- No body opener simply repeats the description.
- `shared-protocol.md` has no duplicate case-state/session-carry sections.
- High-risk vocabulary has canonical wording.
- Gates contain only stage-specific hard stops or true route decisions.
- Handoff consolidation, if done, preserves closure resume semantics.
- `Execution Instruction Order` and `Fact Authority Order` remain separate.
- No workflow behavior, skill ownership, TDD policy, confirmation gate, closure
  status meaning, or Artifact Policy semantics are weakened.

## Rollback Rule

If a cleanup edit makes a rule harder to find at the moment the agent must obey
it, revert that edit and keep the local wording. Token reduction is secondary to
correct skill execution.
