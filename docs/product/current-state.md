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
who needs durable context, explicit confirmations, and clean closure records
without adopting a CLI backend or pipeline product.

## Current Demo State

- Development workflow skills exist for routing, context authority, planning,
  TDD execution, and closure.
- Manual `review` and `grill` skills exist as conversation-only helpers.
- `docs/cases/README.md` provides a derived dashboard for current and recent
  cases.
- `docloom-workflow` can perform read-only next-slice discovery from this file,
  the cases dashboard, closure follow-ups, and targeted repo evidence.
- `skills/_shared/references/loop-protocol.md` defines case candidates,
  next-slice candidates, and the simple scoring output.
- Case artifacts remain the source of truth for routing and evidence.

## Current Bottleneck

Next-slice discovery now exists, but the simple scoring rubric has not yet been
dogfooded on a real small feature. The next bottleneck is proving that candidate
evidence and scoring are useful enough before strengthening the rubric.

## Feedback / Signals

- User explicitly wants AI-assisted discovery of the next most useful
  development slice.
- Prior dashboard integration left next-slice discovery as the next follow-up.
- The next-slice integration case closed with a follow-up to dogfood the rubric.
- The project constitution requires the smallest useful workflow contract and
  rejects heavy orchestration.

## Constraints

- No CLI backend, daemon, scheduler, MCP server, GitHub Actions loop runner, or
  centralized orchestrator.
- Keep `review` and `grill` manual-only.
- Keep `docloom-workflow` a thin router and discovery entry, not an executor.
- Preserve plan confirmation and TDD execution gates.

## Do Not Build Yet

- Product, research, design, release, or operations skill groups.
- Automatic execution of discovered candidates.
- Automatic promotion of candidate recommendations into authority.
- External loop-engineering root scaffolds such as `STATE.md` or `LOOP.md`.
