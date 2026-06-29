# SSOT Map

This map identifies the source of truth for active Doc Loom Least facts.

| Fact area | Source of truth | Notes |
|---|---|---|
| Constitutional principles and conflict handling | [ADR-0000 Constitution](adr/ADR-0000-constitution.md) | Highest authority. |
| Lifecycle scope and meaning of "platform" | [ADR-0001 Lifecycle Scope And Skill Grouping](adr/ADR-0001-lifecycle-scope-and-skill-grouping.md) | Current domain is development; future domains are allowed only by need. |
| Skill physical layout | [ADR-0001](adr/ADR-0001-lifecycle-scope-and-skill-grouping.md), [skills/README](../skills/README.md) | `development/`, `governance/`, `assessment/`, `_shared/`. |
| Install and skillshare behavior | [INSTALL](../INSTALL.md) | Includes expected canonical skills and sync notes. |
| Shared workflow protocol | [shared-protocol](../skills/_shared/references/shared-protocol.md) | Consumed through per-skill symlinks. |
| Per-skill runtime behavior | Each `SKILL.md` under [skills](../skills/) | Frontmatter names define invocation names. |
| User-facing overview | [README](../README.md), [README_CN](../README_CN.md) | Entry points only; revise if they conflict with ADRs or skill files. |
