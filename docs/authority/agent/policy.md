---
status: active
authority: true
layer: authority
type: agent
source_of_truth: adr
supersedes: []
superseded_by: []
owner: user
last_verified: 2026-07-12
---

# Agent Policy

Agents working in this repository must follow the constitution first. If a
lower-authority document conflicts with the constitution, revise, supersede, or
block the lower-authority document for owner decision.

Human experience comes first. Agents should own discoverable and executable
workflow work inside the repository and available tools, including context
gathering, validation, routine command execution, document updates, and
bookkeeping. Interrupt the user only for real human decisions, external facts,
explicit authorization, preferences, or high-risk irreversible actions.

Agent adapters such as `AGENTS.md` and `CLAUDE.md` are derived entry points.
They should remain thin pointers to this policy, the constitution, and current
authority docs. When the active constitution changes, update adapters to point
to it; do not copy constitutional clauses or the full protocol into adapters.

## High-Risk Topics And Classification

Security, auth, permission, privacy, billing, data deletion, public API/CLI,
schema, config contract, migration, ADR-protected architecture boundaries, and
workflow or agent policy require deliberate context checks.

Execution risk is classified from consequence, reversibility, exposure,
authority impact, and verification strength. Security, money, private data,
destructive operations, irreversible migration, and public compatibility are
presumptively high. Reversible internal workflow or Skill text is normally
medium, but remains high when it weakens authorization, changes a public
contract or L1 critical fact, or enables irreversible action.

## Sources

- [Constitution](../constitution.md)
- [ADR-0002 Human-First Agent Responsibility](../../adr/ADR-0002-human-first-agent-responsibility.md)
- `skills/_shared/references/shared-protocol.md`
- `skills/governance/setup-doc-governance/references/layering-and-routing.md`
