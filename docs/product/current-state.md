+# Product Current State

This file is an operational input for discovery, not product authority.

## Goal

Doc Loom Least is a small, Markdown-first personal development workflow for
using AI agents with durable context and proportionate verification.

## Current behavior

- docloom-workflow is the normal entry and owns context checks, authorization,
  execution, verification, optional task.md, status, continuation, discovery,
  and narrow documentation sync.
- review is a separate read-only pass for explicit requests or defined
  development triggers; it does not require another agent.
- grill is an explicit conversational challenge and never changes files or state.
- setup-doc-governance handles structural documentation and authority changes.
- business-docs independently organizes business rules and decisions from
  conversations, tasks, and historical evidence; development uses it for
  business conclusions during work and final archival.
- Reversible one-turn work needs no task record. Durable task state uses one
  task.md; business documents can exist independently of it.
- Legacy case artifacts remain readable evidence.
- There is no runtime backend, daemon, orchestrator, or automatic publishing.

## Current bottleneck

Dogfood the five capabilities on ordinary work, a resumed task, a material
review, a governance change, and standalone business documentation. Measure
whether context loss, unnecessary confirmation, false completion, unsupported
business rationale, and record churn remain acceptable.

## Do not build yet

Broader product lifecycle machinery, speculative lifecycle domains, a runtime
workflow engine, automatic candidate execution, or global installation management.
