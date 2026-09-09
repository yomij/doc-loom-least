---
status: active
authority: true
layer: authority
type: workflow
source_of_truth: code
supersedes: []
superseded_by: []
owner: user
last_verified: 2026-09-09
---

# Development Workflow

The normal development entry is docloom-workflow. Its default behavior is:

1. Understand the requested outcome from current authority, repository
   instructions, implementation evidence, and the actual worktree.
2. Execute within authorization, choosing files, tools, sequence, tests, and
   commits as needed.
3. Verify each success condition with credible evidence.
4. Update affected current or derived documentation and report the result.

Reversible one-turn work needs no case. Create docs/cases/<task-id>/task.md only
when work must survive the turn, preserve a durable decision, handle a meaningful
interruption, or the user asks for a case. The record carries current state,
verification evidence, and final result; attachments never own status.

Ask only for an unresolved material fact, a changed outcome or constraint, an
action outside authorization, or a consequential decision. Necessary lockfile
or dependency changes inside an authorized outcome do not require a second
confirmation merely because of their file type.

Use review for an explicit request or when consequence, compatibility,
permission, governance, public contract, or weak evidence warrants a read-only
independent check. Use grill only when the user explicitly asks to challenge a
claim. Use setup-doc-governance for structural authority changes and conflicts.

On resume, read task.md and the minimum changed evidence; current intent and
authorization must still apply. Time alone never expires or extends authority.
Legacy plan, execution, closure, and handoff artifacts remain historical
evidence and are not batch-migrated.

Task state is active, paused, blocked, done, cancelled, superseded, or abandoned;
paused and blocked records name the reason and next action. Completion requires
credible evidence for every stated success condition.
Unverified work is reported as unverified. Durable status is carried by task.md
for new work, not inferred from several report files.
