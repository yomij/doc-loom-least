---
title: Doc Loom Skills v1 Clean Independent Plan
type: implementation-plan
status: proposed
created: 2026-06-23
scope:
  - skills
  - shared-protocol
  - references
  - templates
tags:
  - doc-loom
  - skills
  - v1-clean
  - prose-cleanup
  - minimum-path
---

# Doc Loom Skills v1 Clean Independent Plan

## Goal

Reduce redundant prose in the Doc Loom Least skills while preserving behavior.

The cleanup must make each skill easier to trigger, read, and maintain. It must
not change workflow semantics, introduce new stages, add a CLI backend, or make
Doc Loom larger than the governance problem it solves.

## Authority Basis

Use these documents as the decision order for this cleanup:

1. `docs/adr/ADR-0000-constitution.md`
2. `docs/design/document-driven-workflow-skills-final.md`
3. Current files under `skills/`
4. Current templates and references under each skill directory

The constitutional standard is minimum path and human-semantic design. Therefore
this plan prefers deletion, consolidation, and clearer ownership over new
frameworks or extra process.

## Scope

In scope:

- `skills/*/SKILL.md`
- `skills/_shared/references/shared-protocol.md`
- Skill-specific reference files under `skills/*/references/`
- Handoff templates under `skills/*/templates/`

Out of scope:

- New skills
- New workflow phases
- CLI backend behavior
- Runtime adapters
- Changes to project-level governance principles
- Changes to task behavior, confirmation gates, TDD policy, or closure status
  meaning

## Current Problems

The skill set is coherent, but several text patterns increase context cost:

- Frontmatter descriptions mix trigger conditions with long feature lists.
- Opening sentences often repeat the description.
- Gates sections restate body rules using route syntax.
- High-risk topic lists appear in multiple files with slightly different
  wording.
- `shared-protocol.md` has internal overlap around case state, session carry,
  artifact policy, resume, and routing.
- A few local references repeat rules already owned by shared protocol.

## Cleanup Principles

1. Preserve every behavioral rule in at least one authoritative location.
2. Keep a rule where the agent needs it at the decision moment.
3. Put cross-skill vocabulary and ownership in shared protocol.
4. Put stage-specific execution details in the owning skill.
5. Delete repeated examples when the rule is already clear.
6. Replace repeated lists with a canonical named reference only when the reading
   skill already loads that reference.
7. Do not make a structural split until text-only cleanup has been verified.

## File Responsibility Map

| File area | Responsibility after cleanup |
|---|---|
| `skills/*/SKILL.md` | Trigger boundary, stage workflow, stage-owned gates, output contract |
| `skills/_shared/references/shared-protocol.md` | Cross-skill ownership, case state, artifact policy, confirmation semantics, route output, high-risk topics |
| `context-authority/references/*` | Context intent and retrieval routing only |
| `setup-doc-governance/references/*` | Governance layering, verdicts, authority routing |
| `doc-sync-close/references/doc-update-rules.md` | Closure-time doc update and narrow authority patch rules |
| `development/tdd-execute/references/tdd-exceptions.md` | TDD exception categories and alternative verification |
| `templates/` | Output shape only, not policy |

## Phase 0: Baseline Inventory

Purpose: capture the current shape before editing.

Steps:

1. Record line counts:

   ```bash
   rtk wc -l skills/*/SKILL.md skills/_shared/references/shared-protocol.md skills/*/references/*.md skills/*/templates/* 2>/dev/null
   ```

2. Record all duplicated high-risk wording:

   ```bash
   rtk rg -n "security|auth|permission|privacy|billing|data deletion|public API|CLI|schema|config contract|ADR-protected" skills
   ```

3. Record all route-style gates:

   ```bash
   rtk rg -n "Route:|Required input|no .* no|Do not" skills/*/SKILL.md
   ```

4. Confirm there are no unreviewed generated artifacts in the target files:

   ```bash
   rtk git status --short -- skills docs/design/v1-clean
   ```

Output: a short baseline note in the implementation session, not a new
artifact.

## Phase 1: Trim Skill Descriptions

Purpose: make descriptions reliable trigger metadata instead of body summaries.

Rule: each `description` should answer only "when should this skill load?"
Prefer one sentence, maximum two short sentences.

Target descriptions:

| Skill | Replacement description |
|---|---|
| `docloom-workflow` | `Default entry for Doc Loom Least. Use when a user asks for a docs-first workflow but does not name a specific Doc Loom skill.` |
| `context-authority` | `Gather minimal context and resolve authority before planning. Use before plan-confirm when a case needs a context gate, on resume, or when authority conflicts may affect the plan.` |
| `plan-confirm` | `Create and confirm an implementation plan before execution. Use after case identity exists and context is gathered or explicitly skipped.` |
| `tdd-execute` | `Execute an approved Doc Loom case plan using TDD discipline. Use only after plan-confirm has produced an approved plan.` |
| `doc-sync-close` | `Sync docs and close a Doc Loom case. Use after execution, or when a case is blocked, cancelled, paused, superseded, or abandoned.` |
| `review` | `Read-only evidence review of a design doc, proposal, code diff, test set, documentation change, or case evidence. Use only when the user explicitly asks for review.` |
| `grill` | `Interactively pressure-test a claim. Use only when the user explicitly asks to grill, challenge, scrutinize, or question assumptions.` |
| `setup-doc-governance` | `Set up or rebuild documentation governance for a project. Use when the user asks to initialize, organize, rebuild, or repair docs governance.` |

Verification:

```bash
rtk rg -n "^description:" skills/*/SKILL.md
```

Expected result: every description is concise and trigger-oriented.

## Phase 2: Remove Duplicate Openers

Purpose: avoid repeating frontmatter in the first paragraph.

Delete or compress body openers that merely restate the new description:

| File | Action |
|---|---|
| `skills/development/docloom-workflow/SKILL.md` | Keep "Route; do not replace stage skills." Remove broad restatement. |
| `skills/development/context-authority/SKILL.md` | Remove "Gather the smallest relevant context needed before planning." Keep read-only boundary. |
| `skills/development/plan-confirm/SKILL.md` | Remove "Create the execution plan and get user confirmation..." Keep case identity boundary. |
| `skills/development/tdd-execute/SKILL.md` | Remove "Execute an approved persistent Doc Loom case plan." Keep one-shot exclusion. |
| `skills/development/doc-sync-close/SKILL.md` | Remove the close-state restatement. |
| `skills/assessment/review/SKILL.md` | Keep explicit manual trigger and read-only boundary because this is the key safety rule. |
| `skills/assessment/grill/SKILL.md` | Keep explicit manual trigger because this is the key safety rule. |
| `skills/governance/setup-doc-governance/SKILL.md` | Keep the authority-vs-history distinction because it is semantic, not duplicate filler. |

Verification:

```bash
rtk sed -n '1,30p' skills/development/docloom-workflow/SKILL.md
rtk sed -n '1,30p' skills/development/context-authority/SKILL.md
rtk sed -n '1,30p' skills/development/plan-confirm/SKILL.md
rtk sed -n '1,30p' skills/development/tdd-execute/SKILL.md
rtk sed -n '1,30p' skills/development/doc-sync-close/SKILL.md
```

Expected result: each body starts with unique operating guidance, not a
description echo.

## Phase 3: Consolidate Shared Protocol Internals

Purpose: remove duplication without changing reference topology.

Make these changes in `skills/_shared/references/shared-protocol.md`:

1. Add a canonical `## High-Risk Topics` section near fact authority:

   ```markdown
   ## High-Risk Topics

   High-risk topics: security, auth, permission, privacy, billing, data deletion,
   public API, CLI, schema, config contract, and ADR-protected architecture
   boundaries.
   ```

2. Merge `## Case State Conflicts` into `## case_state.yaml`.

   Keep the phase derivation order:

   ```text
   closure.md exists -> closed
   execution.md exists and acceptance checks are complete -> doc_syncing
   execution.md exists -> executing
   plan.md has status: approved -> planned
   plan.md has status: draft -> waiting_for_plan_confirmation
   no clear signal -> needs_user_decision
   ```

3. Delete `## Session State Carry` after moving its useful sentence into
   `## case_state.yaml`.

4. Merge the split `closure.md` row back into the main Artifact Policy table.

5. Compress `## Skill Runtime Boundary` to two bullets:

   ```markdown
   Do not:
   - Depend on a CLI backend or central task indexes.
   - Replace stage skills from the entry skill.
   ```

Do not split `shared-protocol.md` in this phase. A split is only justified if
the verified cleaned file still causes repeated unnecessary loading.

Verification:

```bash
rtk rg -n "Session State Carry|Case State Conflicts|closure.md" skills/_shared/references/shared-protocol.md
rtk rg -n "High-Risk Topics|High-risk topics" skills/_shared/references/shared-protocol.md
```

Expected result:

- `Session State Carry` heading is gone.
- `Case State Conflicts` heading is gone.
- `closure.md` appears in the main Artifact Policy table.
- `High-Risk Topics` appears exactly once.

## Phase 4: Replace Repeated High-Risk Lists

Purpose: avoid wording drift while preserving standalone clarity.

Use the shared protocol canonical list when the skill already reads
`references/shared-protocol.md`:

| File | Action |
|---|---|
| `skills/development/context-authority/SKILL.md` | Replace the inline high-risk conflict list with a reference to `shared-protocol.md -> High-Risk Topics`; keep code/tests disagreement and missing coverage bullets if they are stage-specific. |
| `skills/assessment/review/SKILL.md` | Replace repeated topic list with `High-Risk Topics`; keep the review-specific evidence requirement. |
| `skills/assessment/grill/SKILL.md` | Replace `## High-Risk Topics` list with a reference to `High-Risk Topics`; keep lower-certainty and evidence-source guidance. |
| `skills/governance/setup-doc-governance/references/verdicts.md` | Keep a local list if this file is used independently, but match the exact shared wording. |
| `skills/development/doc-sync-close/references/doc-update-rules.md` | Replace high-risk wording in narrow patch conditions with a reference to `High-Risk Topics`. |

Verification:

```bash
rtk rg -n "security, auth|public API, CLI|ADR-protected" skills
```

Expected result: only the canonical shared list and any explicitly justified
standalone reference copy remain.

## Phase 5: Deduplicate Gates

Purpose: make Gates only capture hard stop conditions and routing decisions.

General rule:

- Keep a Gate when it decides whether the current skill may proceed.
- Keep route syntax only when another skill or user input is actually required.
- Delete Gates that restate body rules without adding a stop condition.

Planned changes:

| File | Keep | Delete or merge |
|---|---|---|
| `docloom-workflow/SKILL.md` | case creation boundary, no auto-execute, no auto-review, no CLI backend | Route Table restatements that add no new condition |
| `context-authority/SKILL.md` | no context, blocking conflict, no case creation, low-authority/stale source risk | broad "do not modify any artifact" list, replaced by one read-only sentence |
| `plan-confirm/SKILL.md` | missing case identity, missing context, blocking conflict, no confirmation | duplicated phase-setting and plan-quality statements |
| `tdd-execute/SKILL.md` | no approved plan, unconfirmed plan version, failed Red, TDD exception missing verification, serious deviation | non-goal and file-scope restatements already covered by Preflight and Adaptive Execution |
| `doc-sync-close/SKILL.md` | authority confirmation, evidence gap, closure ordering | acceptance mapping restatements |
| `review/SKILL.md` | explicit review request, missing evidence cannot pass | repeated no-write/no-route bullets already in opening boundary |
| `grill/SKILL.md` | explicit grill request, one question at a time, no unconfirmed decision promotion | repeated no-file/no-artifact bullets |
| `setup-doc-governance/SKILL.md` | confirmation gate, existing target path, scope default | authority rule restatements already in Authority Rules |

Verification:

```bash
rtk rg -n "## Gates|Route:|Required input" skills/*/SKILL.md
```

Expected result: Gates sections are shorter and route syntax appears only where
the next action is clear.

## Phase 6: Local Skill Cleanup

Purpose: remove stage-specific redundancy that is not solved by shared protocol.

Changes:

1. `skills/development/docloom-workflow/SKILL.md`
   - Merge Route Table rows that both route to `context-authority`.
   - Keep fast-path rules by reference to shared protocol.

2. `skills/development/context-authority/references/context-resolution.md`
   - Delete duplicate resume-from-closed guidance.
   - Keep one reference to shared protocol case identity and closure status.

3. `skills/development/context-authority/references/retrieval-routing.md`
   - Convert governance plan reading rules into a short priority list.
   - Keep missing-governance outcomes.

4. `skills/development/plan-confirm/SKILL.md`
   - Replace long version and confirmation prose with a state-transition table.
   - Keep material-change definition.

5. `skills/development/tdd-execute/SKILL.md`
   - Merge `Review Risk` and `Review Risk Lifecycle` into one compact section.
   - Compress testing anti-pattern bullets into two sentences.
   - Keep Red/Green/Refactor evidence requirements intact.

6. `skills/development/doc-sync-close/SKILL.md`
   - Delete examples under Knowledge Changes.
   - Keep the classification list and authority handling rule.

7. `skills/governance/setup-doc-governance/SKILL.md`
   - Replace the negative artifact-name list with a reference to Artifact Policy.
   - Keep confirmation-before-apply semantics.

Verification:

```bash
rtk rg -n "Resume-from-closed|Review Risk Lifecycle|FACT_REGISTRY|KNOWLEDGE_CARDS|CONFLICT_REPORT|AUTHORITY_REBUILD_PLAN|CONTEXT_PACK_POLICY" skills
```

Expected result: repeated or legacy-warning text is gone, but policy ownership
is still visible through shared protocol or the owning section.

## Phase 7: Handoff Template Consolidation

Purpose: reduce repeated template files only if the content is materially the
same.

Steps:

1. Compare existing handoff templates:

   ```bash
   rtk diff -u skills/development/plan-confirm/templates/handoff.md skills/development/tdd-execute/templates/handoff.md
   rtk diff -u skills/development/tdd-execute/templates/handoff.md skills/development/doc-sync-close/templates/handoff.md
   ```

2. If differences are only headings or phase labels, create:

   ```text
   skills/_shared/templates/handoff.md
   ```

3. Use this shared template:

   ```markdown
   # Handoff

   ## Current Phase

   ## Last Completed

   ## Next Step

   ## Do Not Touch

   ## Commands Last Run

   ## Known Issues

   ## Resume Condition

   ## Source of Detailed History
   ```

4. Update `plan-confirm`, `tdd-execute`, and `doc-sync-close` references to the
   shared template.

5. Delete only the redundant per-skill handoff templates.

If any template carries unique stage semantics, do not consolidate it.

Verification:

```bash
rtk rg -n "templates/handoff.md|_shared/templates/handoff.md" skills
rtk test -f skills/_shared/templates/handoff.md
```

Expected result: either one shared handoff template exists and all references
point to it, or per-skill templates remain because unique semantics were found.

## Phase 8: Optional Shared Protocol Split

Purpose: reduce loaded context only if text-only cleanup is not enough.

Do not run this phase by default.

Trigger this phase only when, after Phases 1-7:

- `shared-protocol.md` is still too broad for common reads, and
- at least three skills regularly need different small subsets, and
- splitting does not make references harder to follow.

Candidate split:

| New file | Owns |
|---|---|
| `skills/_shared/references/routing.md` | skill ownership, route output, explicit manual helpers |
| `skills/_shared/references/case-state.md` | case identity, case_state.yaml, conflict derivation, resume |
| `skills/_shared/references/artifact-policy.md` | artifact table, case creation, fast-path |
| `skills/_shared/references/confirmation.md` | confirmation semantics, high-risk topics, fact authority |

If this split is performed, update every `Read when trigger condition is met`
entry in each skill. Run a full reference check afterward.

## Verification Plan

Run after each phase:

```bash
rtk rg -n "references/shared-protocol.md|_shared/references|templates/handoff.md|High-Risk Topics|Route:" skills
rtk git diff -- skills
```

Run after all phases:

```bash
rtk rg --files skills
rtk rg -n "T[B]D|T[O]DO|f[i]ll in|implement late[r]" skills docs/design/v1-clean
rtk rg -n "description:" skills/*/SKILL.md
rtk rg -n "status: needs_refresh|in_review" skills
```

Manual review checklist:

- Every skill still has a clear trigger boundary.
- `review` and `grill` remain explicit user-only helpers.
- `docloom-workflow` remains a thin router and case identity owner.
- `plan-confirm` remains the only plan confirmation gate.
- `tdd-execute` still requires approved plan and observed Red when TDD applies.
- `doc-sync-close` still cannot mark unmet acceptance as unqualified `Done`.
- Authority changes still require explicit user confirmation unless they are
  confirmed narrow patches.
- No deleted template or reference is still mentioned by a skill.

## Acceptance Criteria

The cleanup is complete when:

- All eight descriptions are concise trigger metadata.
- No skill body opens by repeating its description.
- Gates sections contain only stage-specific hard stops or true route decisions.
- `shared-protocol.md` has no duplicate session carry or case conflict section.
- High-risk topics have one canonical wording.
- `context-resolution.md` no longer repeats resume-from-closed guidance.
- `tdd-execute` has one review-risk section, not three overlapping sections.
- Handoff templates are either consolidated or explicitly left separate because
  they contain unique semantics.
- The net skill text is shorter by at least 20% without behavioral loss.
- A final `git diff -- skills` review shows only prose consolidation and
  reference updates, not new workflow behavior.

## Rollback Rule

If a cleanup edit makes a rule harder to find at the decision moment, revert
that edit and keep the clearer local wording. Token reduction is secondary to
correct skill execution.
