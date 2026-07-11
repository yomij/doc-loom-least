# SSOT Map

This map identifies the source of truth for active Doc Loom Least facts.

| Fact area | Source of truth | Notes |
|---|---|---|
| Authority entry | [Authority Index](authority/README.md) | Current governed fact route. |
| Constitutional principles and conflict handling | [Constitution](authority/constitution.md), [Product Scope](authority/product/scope.md) | `docs/authority/constitution.md` is the active constitutional authority. |
| Lifecycle scope and meaning of "platform" | [Product Scope](authority/product/scope.md), [ADR-0001](adr/ADR-0001-lifecycle-scope-and-skill-grouping.md) | Current domain is development; future domains are allowed only by need. |
| Human-first workflow and agent responsibility | [Agent Policy](authority/agent/policy.md), [ADR-0002](adr/ADR-0002-human-first-agent-responsibility.md) | Authority summarizes the active policy; ADR records the decision. |
| Skill physical layout | [Repository And Skills](authority/architecture/repo-and-skills.md) | `development/`, `governance/`, `assessment/`, `_shared/`. |
| Install and skillshare behavior | [Distribution](authority/operations/distribution.md), [INSTALL](../INSTALL.md) | Includes expected canonical skills and sync notes. |
| Git commit message standard | [Git Commit Titles](authority/operations/git.md) | Active commit-title rule. |
| Shared workflow protocol | [Development Workflow](authority/workflow/development-flow.md), [shared-protocol](../skills/_shared/references/shared-protocol.md) | Current implementation remains direct evidence. |
| Loop-style discovery protocol | [loop-protocol](../skills/_shared/references/loop-protocol.md), [Development Workflow](authority/workflow/development-flow.md) | Implementation guidance for case candidate output and next-slice discovery; not a new workflow stage. |
| Case discovery dashboard | [Cases README](cases/README.md), per-case artifacts under `docs/cases/<case-id>/` | Dashboard is derived; current status comes from `plan.md`, `execution.md`, and `closure.md` artifact precedence. |
| Next-slice product input | [Product Current State](product/current-state.md), active authority docs | Operational discovery input only; authority docs and current implementation win on conflict. |
| Per-skill runtime behavior | Each `SKILL.md` under [skills](../skills/) | Frontmatter names define invocation names. |
| User-facing overview | [README](../README.md), [README_CN](../README_CN.md) | Entry points only; revise if they conflict with ADRs or skill files. |
