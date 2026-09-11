---
status: active
authority: true
layer: authority
type: architecture
source_of_truth: code
supersedes: []
superseded_by: []
owner: user
last_verified: 2026-09-10
---

# Repository And Skills

Doc Loom Least ships as Agent Skills and Markdown. It has five canonical
discoverable Skills:

| Skill | Source | Role |
|---|---|---|
| docloom-workflow | skills/development/docloom-workflow/SKILL.md | Normal development entry and optional durable task record. |
| business-docs | skills/product/business-docs/SKILL.md | Independent business documentation from conversations, tasks, and historical evidence. |
| review | skills/assessment/review/SKILL.md | Separate read-only pass on request or a defined development trigger; no required subagent. |
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
Each Skill carries its routing and core behavior rules. README files are
maintenance navigation, not installed runtime dependencies. Resources load only
under their stated conditions; the five capabilities are not sequential phases.
Future lifecycle groups are added only when a real boundary and useful contract
exist.

ADR-0005 adds the narrow product/business-document capability. Its template is
local to business-docs; docloom-workflow invokes it by Skill name when business
content needs retaining. Standalone business writing needs no task record or
development workflow. Missing integration capability is disclosed rather than
silently treated as completed archival.
