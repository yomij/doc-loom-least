---
title: Governance Plan Batch Lifecycle Proposal
type: proposal
status: proposed
created: 2026-06-23
tags:
  - doc-governance
  - governance-plan
  - artifact-lifecycle
---

# Governance Plan Batch Lifecycle Proposal

## Problem

The current `setup-doc-governance` design writes every written governance task to
one fixed path:

```text
docs/governance/GOVERNANCE_PLAN.md
```

That makes the second independent governance task modify the same file as the
first task. A single file starts to mix prior applied decisions, a new proposal,
blocked items, follow-ups, and execution results from unrelated governance
batches.

This violates the constitution's minimum-path principle when the file becomes a
global running ledger instead of the smallest artifact needed for the current
governance failure.

## Product Decision

Change the product model from:

```text
one repository = one mutable GOVERNANCE_PLAN.md
```

to:

```text
one independent governance batch = one governance plan file
```

Within the same governance batch, the same plan file may move through:

```text
proposed -> approved -> applied | applied_with_blocks | blocked
```

A second independent governance task must not overwrite or append to the first
task's plan.

## Path Rule

Use the simplest path that fits the batch's context:

```text
If a case is active and the governance is tied to that case:
  docs/cases/<case-id>/governance-plan.md

Otherwise:
  docs/governance/YYYY-MM-DD-<slug>.md
```

Small low-risk governance that only needs 1-3 localized decisions, has no
blocked facts, and introduces no new authority section continues to use
conversation confirmation (the existing fast-path behavior). No plan file is
written for inline-only decisions.

## Deprecated Fixed Path

Deprecate this path as a write target:

```text
docs/governance/GOVERNANCE_PLAN.md
```

If an older project already has that file:

- read it only as legacy context;
- do not write a new independent governance batch into it.

No migration or archival of the legacy file without explicit user confirmation.

## Governance Plan Frontmatter

Written governance plans should include batch identity:

```yaml
---
status: proposed
plan_version: 1
scope: docs-only
governance_batch: 2026-06-23-short-slug
approved_by:
approved_at:
---
```

## Required Rule Changes

Update `setup-doc-governance` so it applies these gates:

- Do not apply governance changes before user confirmation.
- Do not write a new independent batch into an existing plan file.
- Continue editing an existing plan only when the new request is clearly the
  same governance batch.
- If batch identity is ambiguous, stop and ask whether to continue the old
  batch or create a new one.
- If `docs/governance/GOVERNANCE_PLAN.md` already exists from a prior design,
  read it as legacy context and write new batches to the date-named path
  instead.
- Small inline decisions must still list file-level and fact-level decisions
  before confirmation.

## Files To Update Later

This proposal intentionally does not change runtime skill behavior yet.

When approved, update:

- `skills/setup-doc-governance/SKILL.md`
- `skills/setup-doc-governance/templates/GOVERNANCE_PLAN.md`
- `skills/context-authority/references/retrieval-routing.md`
- `skills/_shared/references/shared-protocol.md`

## Acceptance Criteria

- A second independent governance task creates a new plan file instead of
  modifying a prior plan.
- `docs/governance/GOVERNANCE_PLAN.md` is no longer the normal write target.
- Existing plans remain traceable by batch.
- User confirmation always names a specific plan file (or confirms inline
  decisions directly).
- Small low-risk governance can still avoid unnecessary files through inline
  confirmation.
