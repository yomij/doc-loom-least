+# Product Current State

This file is an operational input for discovery, not product authority.

## Goal

Doc Loom Least is a small, Markdown-first personal development workflow for
using AI agents with durable context and proportionate verification.

## Current behavior

- docloom-workflow is the normal entry and owns context checks, authorization,
  execution, verification, optional task.md, status, continuation, discovery,
  and narrow documentation sync.
- review is a read-only helper for explicit requests or material risk.
- grill is an explicit conversational challenge and never changes files or state.
- setup-doc-governance handles structural documentation and authority changes.
- Reversible one-turn work needs no record. Durable work uses one task.md.
- Legacy case artifacts remain readable evidence.
- There is no runtime backend, daemon, orchestrator, or automatic publishing.

## Current bottleneck

Dogfood the four-Skill flow on ordinary work, a resumed task, a material review,
and a governance change. Measure whether context loss, unnecessary confirmation,
false completion, and record churn remain acceptable.

## Do not build yet

New lifecycle domains, a runtime workflow engine, automatic candidate execution,
or global installation management.
