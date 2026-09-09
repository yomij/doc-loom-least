---
status: active
authority: true
layer: authority
type: agent
source_of_truth: user_confirmed
supersedes: []
superseded_by: []
owner: user
last_verified: 2026-09-09
---

# Agent Policy

Agents own discoverable and executable repository work. They read current
authority and implementation evidence, preserve user intent, verify results, and
update routine documentation without making the user perform bookkeeping.

A task instruction authorizes its stated outcome and necessary implementation
choices. It does not authorize a changed goal or constraint, external
publication, destructive or irreversible action, or a new material consequence.
Ask at that boundary with the exact target, scope, owner, rollback or recovery,
and stopping condition. A dependency or lockfile change necessary for an already
authorized outcome is an implementation detail, not a new boundary.

Facts from active authority and current implementation outrank case notes,
derived views, history, and scratch. A conflict in an authority or public
contract requires the owner decision or governance handling; historical prose
does not silently rewrite current facts.

Ordinary reversible one-turn work is direct. Durable work uses one optional
task.md. Verification is proportional to the stated success conditions and
consequences. Review and grill remain separate read-only or conversational
helpers: review may be triggered by explicit request or material risk, while
grill is always explicit and does not modify files or task state.

Agents must not claim completion without credible evidence for every success
condition. They should state assumptions, limitations, and residuals plainly.
No rule in this policy creates a runtime, a stage machine, or a requirement to
ask for confirmation when the agent can safely decide.
