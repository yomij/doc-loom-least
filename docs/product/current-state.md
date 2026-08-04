# Product Current State

This file is an operational input for next-slice discovery. It is not product
authority. Active authority docs and current skill implementation win on
conflict.

## Product Goal

Doc Loom Least is a minimal, document-driven personal product workflow
substrate. The current usable slice is AI-assisted development through Agent
Skills and Markdown artifacts.

## Target User

A solo builder or small-team maintainer using AI agents for development work
who needs durable context and guarded confirmation only when consequences call
for them, without adopting a CLI backend or pipeline product.

## Current Demo State

- Development workflow skills exist for routing, context authority, planning,
  TDD execution, and closure.
- Direct reversible one-turn low/medium work runs without a case; compact
  persistent cases retain concise plan/closure evidence; guarded work adds
  explicit confirmation and exact-baseline review.
- `review` supports explicit ad-hoc assessment and selectively triggered
  workflow-owned Post-execution Engineering/Spec review; `grill` remains
  conversation-only.
- Execution artifacts and semantic commits are conditional. Unqualified `Done`
  waits for commit success only when the approved plan declared that commit.
- `docs/cases/README.md` provides a derived dashboard for current and recent
  cases.
- `docloom-workflow` can perform read-only next-slice discovery from this file,
  the cases dashboard, closure follow-ups, and targeted repo evidence.
- `skills/_shared/references/loop-protocol.md` defines case candidates,
  next-slice candidates, and the compact scoring output.
- Case artifacts remain the source of truth for routing and evidence.

## Current Bottleneck

Real-project evidence showed workflow ceremony dominating ordinary delivery:
51 sampled cases contain 10,614 lines of case Markdown, 39/47 classified plans
are medium risk, and none used the former numeric shortcut. The proportional
model now needs dogfooding to confirm that direct/compact work reduces plan
revisions, case-document churn, and bookkeeping-only commits while guarded
work keeps its assurance.

## Feedback / Signals

- User explicitly wants AI-assisted discovery of the next most useful
  development slice.
- Prior dashboard integration left next-slice discovery as the next follow-up.
- The next-slice rubric dogfood found that the default candidate table exposed
  too much scoring detail.
- Compact candidate output keeps score factors for ranking but hides them from
  the default decision view.
- The project constitution requires the smallest useful workflow contract and
  rejects heavy orchestration.
- Real case history showed that medium risk commonly paid the full persistent
  workflow cost and that numeric shortcut eligibility did not translate into
  actual usage.

## Constraints

- No CLI backend, daemon, scheduler, MCP server, GitHub Actions loop runner, or
  centralized orchestrator.
- Keep ad-hoc `review` and `grill` manual-only; keep triggered Post-execution
  review inside `tdd-execute`, without a new phase or artifact.
- Keep `docloom-workflow` a thin router and discovery entry, not an executor.
- Preserve explicit current-plan confirmation for guarded work and TDD or its
  recorded verification exception for case execution.

## Do Not Build Yet

- Product, research, design, release, or operations skill groups.
- Automatic execution of discovered candidates.
- Automatic promotion of candidate recommendations into authority.
- External loop-engineering root scaffolds such as `STATE.md` or `LOOP.md`.
