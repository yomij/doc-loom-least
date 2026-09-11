---
status: active
authority: true
layer: authority
type: workflow
source_of_truth: code
supersedes: []
superseded_by: []
owner: user
last_verified: 2026-09-10
---

# Development Workflow

The normal development entry is docloom-workflow, including ordinary document
edits. It establishes the outcome from instructions and evidence, executes
within authorization, verifies success conditions, updates affected documents,
and reports results. Users do not select a phase.

Reversible one-turn work needs no case. Create docs/cases/<task-id>/task.md only
for continuity, a reusable decision, an interrupted handoff, or an explicit case
request. It owns current status; attachments provide evidence only. Load its
local template when creating or restructuring the record, not on every update.

Business persistence is independent of task-state persistence. Invoke
business-docs when requirements create or clarify business rules or decisions,
record meaningful corrections during work, and finalize the business document
at closure. Report its link, or no business change for technical-only tasks.
No task record is required solely for a business document. If business-docs is
unavailable, retain sourced conclusions in the existing record or response and
report incomplete archival; do not claim full documentation completion.
Standalone business-document requests go directly to business-docs, whose
[contract](business-docs.md) covers conversations and historical investigation.

Ask only when unresolved facts or conflicts change the outcome, constraints,
public contract, permissions, or durable authority, or an action needs new
authorization. Existing authorization covers necessary implementation choices
including dependency/lockfile edits. Unspecified destructive targets require
clarification; concrete destructive, irreversible, permission, data-deletion,
publication, or public-contract changes require authorization if not already
covered. Continue work independent of any pending decision.

Use review for explicit requests, changes to security-sensitive behavior,
authentication or permissions, public API/contracts, data migrations, or shared
deployment/storage/build behavior used by multiple applications. Other ordinary
bug fixes, localized refactors, and small features need local verification only.
A separate read-only pass does not require another agent. Review owns findings;
the executor owns fixes. Use grill only for explicit assumption challenges and
setup-doc-governance for structural authority or hierarchy decisions.

On resume, recover intent from the latest request and task.md, then inspect
changed evidence. Ask only if ambiguity or stale context changes the outcome,
or additional authorization is needed. Time alone neither expires nor extends
authorization. Legacy artifacts remain evidence without default migration.

Task state is active, paused, blocked, done, cancelled, superseded, or abandoned,
with evidence, any blocker, and next action. Done requires verification evidence
for every success condition and no unresolved Critical/Important findings or
conclusion-blocking gaps in required review. Repeat checks only for changed
behavior, failures, or unresolved evidence. Report unverified work as such.
