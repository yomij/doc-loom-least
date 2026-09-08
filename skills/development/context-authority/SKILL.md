---
name: context-authority
description: Check context before planning for resume ambiguity, conflicts, authority/public or workflow/agent-policy changes, high risk, or weak evidence. Skip routine direct work.
---

# context-authority

Read `references/shared-protocol.md` for authority, identity, risk, and resume
rules. Reuse the router's workspace snapshot or obtain it. This stage reads
context and returns a verdict; it does not plan or create case identity.

For authority, workflow/agent policy, public, or high-risk work, start with the
active constitution and authority index, then applicable approved governance.
Read only relevant implementation/evidence and the current artifacts for an
explicit or resumed case. Historical and derived material remains supporting
context, not current authority.

Resolve an existing case or propose a slug to `docloom-workflow`. Return an
inline summary of material sources, constraints, conflicts, and risk for the
plan owner. Persist `templates/context-authority-brief.md` only for conflict,
explicit request, continuity, or support that must survive independently.

| Verdict | Meaning |
|---|---|
| `proceed_to_plan` | Enough context; no blocking conflict. |
| `proceed_to_plan_with_risk` | A non-blocking gap remains. |
| `needs_user_decision` | A material fact or choice cannot be resolved. |
| `needs_case_selection` | No safe choice among cases. |
| `run_setup_doc_governance` | Governance must be established first. |
| `blocked_by_authority_conflict` | Material current-authority or evidence conflict. |

Blocking conflict prevents planning. Reversible internal drift may proceed
with recorded risk; do not downgrade a consequential evidence gap to do so.
