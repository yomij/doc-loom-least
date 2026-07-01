---
title: Human-First Agent Responsibility
type: adr
status: accepted
created: 2026-07-01
updated: 2026-07-01
tags:
  - doc-loom
  - constitution
  - human-first
  - agent-responsibility
---

# ADR-0002: Human-First Agent Responsibility

## Context

The owner explicitly decided that Doc Loom Least should prioritize the human user's experience at the top level.

The workflow should not require users to do unnecessary work when an agent can reasonably complete it. User interruption should be reserved for cases where human judgment, external information, explicit authorization, or high-risk irreversible action is actually required.

## Decision

Amend `docs/adr/ADR-0000-constitution.md` with a constitutional principle: Human Experience Comes First.

Agents should own discoverable and executable workflow work inside the repo and available tools, including context gathering, validation, routine command execution, document updates, and bookkeeping. Users should not be forced to perform those steps unless the agent lacks the required access or the step requires a human decision.

## Consequences

- Skills and workflow documents must prefer agent-owned execution over unnecessary user handoffs.
- Confirmation prompts should be used only when they protect a real decision boundary.
- Process completeness cannot justify adding user burden.
- If a lower-authority document requires unnecessary user work, it should be revised or treated as conflicting with the constitution.
