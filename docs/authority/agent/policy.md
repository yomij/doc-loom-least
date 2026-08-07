---
status: active
authority: true
layer: authority
type: agent
source_of_truth: user_confirmed
supersedes: []
superseded_by: []
owner: user
last_verified: 2026-08-07
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

Risk classification and workflow persistence are independent. Medium risk
alone does not require a case or another confirmation. Create durable workflow
only for continuity, durable human decisions, explicit case requests, or
guarded execution; add guarded assurance for high/public/authority-sensitive,
irreversible/destructive, or weakly verified work.

Plan authorization is outcome-based. The plan body contains only Goal, Success
Criteria, and Constraints; it does not prescribe implementation paths. Agents
own context use, file discovery, task decomposition, commands, sequencing, run
mode, tests, TDD/verification choice, review invocation, and commit
organization inside that contract. They must stop for a Goal, Success
Criterion, or Constraint change; risk escalation; authority/public-contract
effect; dependency/config/CI/schema effect; external resource; irreversible
action; or another protected effect.

## Sources

- [Constitution](../constitution.md)
- [ADR-0002 Human-First Agent Responsibility](../../adr/ADR-0002-human-first-agent-responsibility.md)
- [2026-07-02 Full-Repo Docs Governance](../../governance/2026-07-02-full-repo-docs-governance.md)
- `skills/_shared/references/shared-protocol.md`
- `skills/governance/setup-doc-governance/references/governance-rules.md`
