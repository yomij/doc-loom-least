# Doc Loom Least

Doc Loom Least is a small, Markdown-first development workflow for a solo
builder and an AI agent. It keeps current facts, human decisions, and
verification evidence visible without turning the repository into a pipeline.

## The default loop

Understand the outcome from current authority and the actual worktree. Execute
within authorization. Verify every success condition. Update affected
documentation and report the result.

Most reversible one-turn work needs no case. When continuity, an important
decision, an interruption, or an explicit request makes persistence useful,
create one task.md with the goal, success conditions, constraints and decisions,
current state, next action, verification evidence, and result. Legacy
plan/execution/closure files remain readable historical evidence.

Ask only when missing facts change the outcome or constraints, or an action
needs authorization not already provided. Resume reuses established intent.

## Skills

| Skill | Use |
|---|---|
| docloom-workflow | Normal development entry, optional task record, status, continuation, and discovery. |
| business-docs | Independent business documentation from conversations, completed tasks, and historical logic. |
| review | Separate read-only verification on request or a defined development trigger; no required subagent. |
| grill | Explicit conversational challenge of a claim or assumption. |
| setup-doc-governance | Structural documentation and authority governance. |

The former context-authority, plan-confirm, tdd-execute, and doc-sync-close
entry points are retired by ADR-0004. Their required behavior belongs to
docloom-workflow; old case artifacts remain valid evidence.

## Business documentation

Use business-docs directly to turn a conversation into business rules and
decisions, archive a task's business outcome, or reconstruct historical logic
from documents and relevant implementation evidence. No prior Docloom session
or task.md is required. Documents serve general business readers, including
people without a technical background. For example: "Explain the current
order-cancellation rules so customer service and operations staff can understand
when cancellation is allowed, what happens next, and any unresolved questions."

The business narrative stays free of implementation details; sources, time scope,
and evidence gaps remain traceable. The default development entry captures
business conclusions during work and finalizes them at closure. Technical-only
work needs no empty business archive. Existing placement conventions take
precedence; defaults are a task's business.md or a dated document under
docs/business/archives/, with a thin business index. Archives preserve snapshots
and do not automatically become current authority. See
[the business contract](docs/authority/workflow/business-docs.md).

## Boundaries

There is no CLI backend, daemon, runtime workflow engine, or automatic
publication. Review and grill do not mutate files or task state. Authority
documents contain confirmed reusable facts; derived docs route readers;
archives are historical.

## Installation

Install the five discoverable Skills with skillshare:

    skillshare install github.com/yomij/doc-loom-least --track --json
    skillshare sync

For a private repository, use its SSH URL. Existing installations are not
automatically cleaned up; sync the source and remove retired stale copies using
your normal skillshare setup. See INSTALL.md for update and audit commands.

## Sources

Current authority is indexed in docs/authority/README.md. The constitution is
the highest authority. Historical material lives under docs/archive/.
