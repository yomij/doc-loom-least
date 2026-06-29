---
title: Lifecycle Scope And Skill Grouping
type: adr
status: accepted
created: 2026-06-29
updated: 2026-06-29
tags:
  - doc-loom
  - lifecycle-scope
  - skills
  - skillshare
---

# ADR-0001: Lifecycle Scope And Skill Grouping

## Context

Doc Loom Least currently implements a development workflow. The owner wants the project to become the personal product workflow substrate that can later carry product, research, design, release, operations, and other lifecycle work.

This creates a wording conflict with the original "not a platform" phrasing. The constitutional intent is to reject complex pipeline products, not to forbid a repo-native personal workflow home built from skills and Markdown.

## Decision

Doc Loom Least is positioned as a repo-native, skill-based, Markdown-first personal workflow substrate. The current supported lifecycle domain remains development.

The `skills/` tree is grouped by domain and capability:

| Directory | Role |
|---|---|
| `skills/development/` | Current development case workflow. |
| `skills/governance/` | Documentation and knowledge governance. |
| `skills/assessment/` | Manual review, challenge, and critique helpers. |
| `skills/_shared/` | Shared protocol and templates; not a directly invoked skill. |

Skill names remain stable. Grouping changes physical organization only; users still invoke `docloom-workflow`, `plan-confirm`, `review`, and the other skills by their frontmatter names.

Future lifecycle domains such as `product/`, `research/`, or `design/` should be added only when they contain real skills with clear boundaries. Do not create empty domain directories as roadmap signaling.

## Consequences

- Skillshare must continue discovering all canonical skills from nested directories.
- Cross-skill shared protocol symlinks must stay readable after grouping.
- README, install docs, docs index, and SSOT map must describe the new positioning and layout.
- The system may expand beyond development without becoming a backend, daemon, or pipeline product.
