---
title: Minimal Four-Skill Workflow
type: adr
status: accepted
created: 2026-09-09
updated: 2026-09-09
tags:
  - doc-loom
  - skills
  - workflow
  - simplification
---

# ADR-0004: Minimal Four-Skill Workflow

## Context

The previous development flow exposed separate context, planning, execution,
and closure Skills. Their contracts were useful but forced routing, duplicated
state, and multiple optional artifacts into ordinary work. GPT-5.6 Sol is
capable of choosing context, implementation, and verification, but model output
is non-deterministic; evidence and explicit decision boundaries therefore still
need a small written contract.

## Decision

Retire context-authority, plan-confirm, tdd-execute, and doc-sync-close as
discoverable entry points. Their necessary behavior is owned by
docloom-workflow, whose normal loop is understand, execute, verify, sync, and
report.

Keep review as a read-only helper for explicit or risk-triggered independent
checking. Keep grill as an explicit conversational challenge because the owner
requested it. Keep setup-doc-governance as a separate Skill for structural
authority and conflict work.

New durable work uses one optional task.md. Existing plan, execution, closure,
handoff, and case dashboard files remain historical evidence and are not
batch-migrated. This decision changes the earlier stable-name consequence in
ADR-0001 only for the four retired stage names; lifecycle scope and grouped
physical layout remain in force.

## Consequences

The canonical install set is four Skills. Ordinary work has no phase selection,
case, review report, or bookkeeping commit by default. Risk, authorization,
authority, verification, and resume evidence remain explicit behaviors.
Existing installations require normal user-managed cleanup after sync.

The repository must measure real GPT-5.6 behavior on representative scenarios
before further removing review or adding compensating ceremony. A model run is
sample evidence, not a universal reliability claim.
