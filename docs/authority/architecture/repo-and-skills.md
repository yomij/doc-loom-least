---
status: active
authority: true
layer: authority
type: architecture
source_of_truth: code
supersedes: []
superseded_by: []
owner: user
last_verified: 2026-09-09
---

# Repository And Skills

Doc Loom Least ships as Agent Skills and Markdown. It has four canonical
discoverable Skills:

| Skill | Source | Role |
|---|---|---|
| docloom-workflow | skills/development/docloom-workflow/SKILL.md | Normal development entry and optional durable task record. |
| review | skills/assessment/review/SKILL.md | Read-only review on request or when risk and evidence require it. |
| grill | skills/assessment/grill/SKILL.md | Explicit conversational challenge of a claim or assumption. |
| setup-doc-governance | skills/governance/setup-doc-governance/SKILL.md | Structural documentation and authority governance. |

context-authority, plan-confirm, tdd-execute, and doc-sync-close are retired
entry points as of ADR-0004. Their necessary behavior is owned by
docloom-workflow. Existing case artifacts produced by them remain readable
historical evidence.

There is no runtime source tree, package manifest, workflow interpreter, daemon,
or centralized orchestrator. Physical directories group Skills only. The
default entry does not require users to select a phase. Supporting resources
are local to their owner; no shared protocol or cross-Skill symlink is required.
Future lifecycle groups are added only when a real boundary and useful contract
exist.
