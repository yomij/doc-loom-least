---
status: active
authority: true
layer: authority
type: agent
source_of_truth: user_confirmed
supersedes: []
superseded_by: []
owner: user
last_verified: 2026-09-10
---

# Agent Policy

Agents own discoverable and executable repository work. They preserve user
intent, follow declared authority, inspect implementation evidence, verify
results, and update routine documentation without user bookkeeping.

A task authorizes its stated outcome and necessary implementation choices,
including dependency/lockfile edits. Ask only for unresolved facts or decisions
that change the outcome or constraints, or for actions beyond authorization.
Before an unauthorized destructive, irreversible, permission, data-deletion,
publication, or public-contract change, establish its target, scope, impact,
recovery limits, and stopping condition. Existing explicit authorization remains
valid; vague instructions do not cover unspecified destructive targets.

Follow the repository's authority order and fact ownership. Active rules define
constraints, implementation reveals actual behavior, and case notes, derived
views, history, and scratch cannot independently redefine authority. Use
existing precedence or owner decisions to settle conflicts; request a decision
only when an unresolved conflict affects the outcome or binding rules.

Ordinary reversible one-turn work is direct; durable work uses one optional
task.md. Resume recovers intent from the latest instruction and record without
reconfirmation unless ambiguity, stale context, or new authorization requires it.

Review is a separate read-only pass on request or for the concrete triggers in
the [development workflow](../workflow/development-flow.md); it is not an
automatic phase or subagent requirement.
Grill requires explicit challenge intent and changes no files or task state.
Governance handles structural documentation decisions; existing approval for
concrete authority changes need not be repeated.

Done requires evidence for every success condition and resolution of blocking
findings or conclusion-blocking gaps in required review. State missing evidence
and residuals. No rule creates a runtime, stage machine, or automatic ceremony.
