---
title: Skills Text Cleanup Plan
type: plan
status: proposed
created: 2026-06-23
tags:
  - skills
  - text-cleanup
  - lean-prose
---

# Skills Text Cleanup Plan

## Problem

The 8 Doc Loom skills plus shared-protocol total roughly 18K tokens of
instruction text. A read-through found recurring redundancy: descriptions that
duplicate the body, Gates sections that restate body rules in route syntax,
the same high-risk topic list repeated with slight wording drift, and
shared-protocol sections that overlap internally. The redundancy increases LLM
context cost and slightly hurts readability without adding behavioral fidelity.

The constitution requires minimum-path, human-semantic design. This plan
removes redundant prose while preserving every behavioral rule.

## Scope

All files under `skills/`:

- 8 `SKILL.md` files
- `skills/_shared/references/shared-protocol.md`
- 7 reference files
- Template files (handoff consolidation only)

Out of scope: behavioral changes, new rules, new skills, structural redesign.

## Principle

Every behavioral rule must survive the cleanup. If a rule appears in two
places, keep it in the place where an agent is most likely to read it at the
moment of decision, and remove or cross-reference the duplicate.

## Change Batches

### Batch 1: Trim frontmatter descriptions

Each `description` field should be a single-sentence trigger condition, <= 25
words. Move feature lists into the body (most already exist there).

| File | Current issue | Fix |
|---|---|---|
| `docloom-workflow/SKILL.md` | 60-word description mixing trigger + feature list | Trigger only: "Default entry for Doc Loom Least. Use when a user asks for a docs-first workflow but does not name a specific skill." |
| `context-authority/SKILL.md` | 55-word description | Trigger only: "Gather minimal context and resolve authority before planning. Use before plan-confirm when a case needs a context gate, on resume, or when authority conflicts may affect the plan." |
| `plan-confirm/SKILL.md` | 45-word description listing outputs | Trigger only: "Create and confirm an implementation plan before execution. Use after case identity exists and context is gathered or explicitly skipped." |
| `tdd-execute/SKILL.md` | 55-word description with 13-item feature list | Trigger only: "Execute an approved Doc Loom case plan using TDD discipline. Use only after plan-confirm has produced an approved plan." |
| `doc-sync-close/SKILL.md` | 58-word description listing 7 close states + 10 features | Trigger only: "Sync docs and close a Doc Loom case. Use after execution, or when a case is blocked, cancelled, paused, superseded, or abandoned." |
| `review/SKILL.md` | 45-word description | Trigger only: "Read-only evidence review of a design doc, proposal, code diff, test set, or case evidence. Use only when the user explicitly asks for review." |
| `grill/SKILL.md` | 50-word description | Trigger only: "Interactively challenge and pressure-test a claim. Use only when the user explicitly asks to grill, challenge, scrutinize, or question assumptions." |
| `setup-doc-governance/SKILL.md` | 50-word description | Trigger only: "Set up or rebuild documentation governance for a project. Use when the user asks to initialize, organize, rebuild, or repair docs governance." |

Estimated saving: ~300 tokens.

### Batch 2: Remove duplicated body opening sentences

Several skills open with a sentence that restates the trimmed description.
Delete the redundant opener and start directly at the workflow or first
actionable section.

| File | Opening sentence to remove |
|---|---|
| `doc-sync-close/SKILL.md` | "Synchronize task documents and close, pause, block, cancel, supersede, or abandon an existing Doc Loom case." |
| `plan-confirm/SKILL.md` | "Create the execution plan and get user confirmation before `tdd-execute`." |
| `tdd-execute/SKILL.md` | "Execute an approved persistent Doc Loom case plan." |
| `context-authority/SKILL.md` | "Gather the smallest relevant context needed before planning." |
| `docloom-workflow/SKILL.md` | "Use the minimum path that safely resolves the current request." (move to a one-line note under Route Table if needed) |

Estimated saving: ~80 tokens.

### Batch 3: Deduplicate Gates against body

Strategy: keep `Route: X. Reason: Y. Required input: Z` annotations only at
actual routing decision points. Remove Gates entries that restate body rules
without adding routing information.

| File | Action |
|---|---|
| `review/SKILL.md` | Gates has 6 entries; 4 restate body constraints. Keep only the route entry ("User must explicitly request review"). Remove "Do not write files", "Do not modify code/docs/state/artifacts", "Do not output workflow route", "Do not write authority proposal" — all covered by body. |
| `doc-sync-close/SKILL.md` | Gates has 9 entries. Remove restatements of Acceptance Mapping rules ("Unmet acceptance criteria cannot be Done", "Missing execution.md required by Artifact Policy..."). Keep routing entries for authority confirmation and execution evidence gap. |
| `grill/SKILL.md` | Gates has 7 entries; 4 restate "do not create files / do not modify files". Keep only the route entry ("User must explicitly ask for grilling"). Merge remaining constraints into one line: "Do not create artifacts or modify files." |
| `context-authority/SKILL.md` | Gates has 6 entries. Remove "Do not modify code, tests, authority..." (body already says read-only). Keep routing entries for missing context, blocking conflict, case creation, and low-authority sources. |
| `plan-confirm/SKILL.md` | Gates has 8 entries. Remove "Draft plans awaiting user approval must set phase..." (Workflow step 8 already covers this). Keep routing entries. |
| `tdd-execute/SKILL.md` | Gates has 10 entries. Remove "Do not modify authority docs" and "Do not modify scripts, dependencies..." (body Preflight and TDD Rules cover these). Keep routing entries. |
| `docloom-workflow/SKILL.md` | Gates has 10 entries. Remove entries that restate Route Table rows (status-only, no auto-execute, no auto-review, no grill). Keep base_commit/risk_level ownership, CLI ban, and state-only-mode constraints. |
| `setup-doc-governance/SKILL.md` | Gates has 10 entries. Remove restatements of Authority Rules ("Do not create empty authority sections", "Do not write unconfirmed raw material..."). Keep routing entries for scope default, confirmation gate, and existing plan file. |

Estimated saving: ~500 tokens.

### Batch 4: Consolidate shared-protocol internal redundancy

| Section | Action |
|---|---|
| `## case_state.yaml` + `## Case State Conflicts` | Merge into one section. Keep the 6-step phase derivation list; remove the duplicate paragraph in `## case_state.yaml` that says "Report state_cache_conflict / derive phase / do not overwrite." |
| `## Session State Carry` | Delete. The carry rule ("next skill may use prior output instead of re-reading") is already stated in `## case_state.yaml`. |
| `## Artifact Policy` | The `closure.md` row is split into a second table below the main table. Merge it back into the main table as a regular row. |

Estimated saving: ~150 tokens.

### Batch 5: Remove negative artifact list in setup-doc-governance

Delete this block from `setup-doc-governance/SKILL.md`:

```text
Do not generate default `FACT_REGISTRY.md`, `KNOWLEDGE_CARDS.md`,
`CONFLICT_REPORT.md`, `AUTHORITY_REBUILD_PLAN.md`, or
`CONTEXT_PACK_POLICY.md`.
```

The Artifact Policy in shared-protocol already says "Only create artifacts
listed in the Artifact Policy table." If legacy concern remains, replace with a
one-line comment: `# Do not invent artifact types not in the Artifact Policy.`

Estimated saving: ~30 tokens.

### Batch 6: Unify the high-risk topic list

Define the canonical list once in `shared-protocol.md` under a named heading:

```markdown
## High-Risk Topics

High-risk topics: security, auth, permission, privacy, billing, data deletion,
public API, CLI, schema, config contract, and ADR-protected architecture
boundaries.
```

In other files that currently inline a variant of this list:

- `grill/SKILL.md` — replace inline list with "See shared-protocol High-Risk Topics."
- `review/SKILL.md` — replace inline list with "See shared-protocol High-Risk Topics."
- `context-authority/SKILL.md` — replace inline list with "See shared-protocol High-Risk Topics."
- `setup-doc-governance/references/verdicts.md` — this file is read independently; keep the list but match the canonical wording exactly.

Note: `grill` and `review` are conversation-only skills that do read
shared-protocol. If there is concern about standalone use without
shared-protocol loaded, keep a one-line inline reference instead of a full
list.

Estimated saving: ~100 tokens.

### Batch 7: Consolidate handoff templates

Three near-identical handoff templates exist. Replace with a single template
`skills/_shared/templates/handoff.md` containing only the heading skeleton with
a `phase:` placeholder. Each skill that writes a handoff references this
template.

Remove:
- `skills/development/plan-confirm/templates/handoff.md`
- `skills/development/tdd-execute/templates/handoff.md`
- `skills/development/doc-sync-close/templates/handoff.md`

Create:
- `skills/_shared/templates/handoff.md`

Template content:

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

Update SKILL.md references in `plan-confirm`, `tdd-execute`, and
`doc-sync-close` to point at the shared template.

Estimated saving: ~50 tokens + 2 fewer files.

### Batch 8: Minor prose tightening

Small fixes across files:

- `grill/SKILL.md`: merge three separate "do not create artifacts" statements
  into one.
- `review/SKILL.md`: replace "case_state.yaml is a routing signal, not
  evidence" with "See shared-protocol case_state.yaml."
- `context-authority/SKILL.md`: simplify "Do not modify code, tests, authority,
  governance plan, case plan, execution, closure, or review artifacts." to
  "Read-only; do not modify any file."
- `plan-confirm/SKILL.md`: remove "Plan Quality Standard" section (restates
  Workflow step 6 and Plan Rules).
- `doc-sync-close/SKILL.md`: remove "Write closure.md before updating
  case_state.yaml" from State Update section (Workflow steps 8-9 already
  define the order).

Estimated saving: ~80 tokens.

## Execution Order

Batches are independent and can be applied in any order. Recommended sequence:

1. Batch 1 (descriptions) — sets the tone for body trimming.
2. Batch 2 (opening sentences) — natural follow-up to Batch 1.
3. Batch 3 (Gates dedup) — largest single saving.
4. Batch 4 (shared-protocol) — internal cleanup.
5. Batch 6 (high-risk list) — depends on Batch 4 for canonical placement.
6. Batch 5, 7, 8 — independent small fixes.

## Verification

After each batch:

1. `rg` for removed text to confirm no orphaned references remain.
2. Read each modified SKILL.md end-to-end to confirm no behavioral rule was
   lost.
3. Confirm all `Read when trigger condition is met` cross-references still
   point at files that exist.
4. Confirm Route Table in `docloom-workflow` still covers every routing
   scenario after Gates trimming.
5. Run any existing evals if present.

## Acceptance Criteria

- Every behavioral rule present before cleanup is still present after cleanup,
  in at least one location.
- No SKILL.md contains a body opening sentence that duplicates its trimmed
  description.
- No Gates section contains an entry that restates a body rule without adding
  routing information.
- `shared-protocol.md` has no internally duplicated section.
- The high-risk topic list is defined once in shared-protocol and referenced
  elsewhere with consistent wording.
- Handoff templates are consolidated to one shared file.
- Total token reduction is approximately 25-35% of original skills text.
