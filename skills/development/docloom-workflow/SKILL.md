---
name: docloom-workflow
description: Handle repository development work from implementation through verification, including ordinary documentation edits. Use when a request asks to build, fix, refactor, or update project files.
---

# Doc Loom Workflow

Use the user's language for all output, including templates, unless another language is requested.

Work directly toward the user's outcome; do not ask the user to choose a phase.

## Work

- Establish the outcome, success conditions, and constraints from the request,
  repository instructions, worktree, and evidence. Follow declared fact ownership:
  active owner-approved rules constrain work; implementation shows behavior.
  Cases, derived views, history, and scratch do not establish authority.
  Do not elevate implementation preferences into requirements.
- Resolve conflicts using declared precedence and existing owner decisions.
  Ask only if an unresolved conflict or missing fact changes the outcome,
  constraints, public contract, permissions, or durable authority. Continue
  independent work while that decision is pending.
- Execute within existing authorization. Choose files, tools, sequence, tests,
  and necessary dependency/lockfile changes. Before an unauthorized destructive,
  irreversible, permission, data-deletion, publication, or public-contract
  change, make its target, scope, impact, recovery limits, and stopping condition
  concrete and obtain authorization. Reuse existing approval; vague requests do
  not authorize unspecified destructive targets.
- Update affected documentation within existing rules. Do not promote an
  inference into authority or restructure governance as ordinary documentation
  maintenance. Discovery-only requests produce an evidence-based recommendation,
  tradeoff, and uncertainty; they do not authorize implementation.
- When requirements establish or clarify business rules or decisions, use
  business-docs to capture them and meaningful corrections during work. At
  closure, finalize its business document and report the link. Check each task
  for business content; technical-only work reports no business change and
  needs no empty archive. Invoke by Skill name, not a sibling-directory path.
  If business-docs is unavailable, preserve the business conclusions and source
  gaps in the existing record or response, disclose incomplete archival, and
  do not silently claim that documentation is complete or install globally.

## Verification

Check each success condition with the smallest check that demonstrates it.
Report the checks, results, and missing evidence. Repeat checks only for changed
behavior, failures, or unresolved evidence gaps.

Invoke review for an explicit request, security-sensitive behavior changes
(such as secret handling or untrusted input execution), authentication or
permission changes, public API/contract changes, data migrations, or changes to
shared deployment, storage, or build behavior used by multiple applications.
Ordinary bug fixes, localized refactors, and small features outside these
conditions need local verification only. Review requires no additional agent.

Finish when every success condition has evidence and required review has no
unresolved Critical/Important findings or conclusion-blocking gaps. Report the
outcome, residuals, and anything unverified or blocked.

## Persistence

Create `docs/cases/<task-id>/task.md` only for continuity beyond this turn,
reusable decisions that need recording, interruption requiring a handoff, or an
explicit case request. Reversible one-turn work needs no task record by default;
business documentation is independent and follows business-docs when relevant.
Read [templates/task.md](templates/task.md) **conditionally**, when creating or
restructuring that record. Existing records can be updated without reloading it.
The record owns status; separate logs and reviews are evidence only.
Link any business document from the record without duplicating its body.

On resume, recover intent from the latest user instruction and task record,
then inspect changed worktree and relevant evidence. Ask only if ambiguity or
stale context changes the outcome, or additional authorization is needed.
Elapsed time alone neither expires nor extends authorization. Keep legacy
plan/execution/closure files as evidence without migrating them by default.

Status is active, paused, blocked, done, cancelled, superseded, or abandoned,
with evidence, any blocker, and next action. Done requires verification above.

## Escalation

- review owns findings; this executor owns authorized fixes and durable status.
- business-docs owns standalone business writing, task business archives, and
  historical business reconstruction; it does not require this workflow.
- grill applies only to an explicit request to challenge assumptions or claims.
- setup-doc-governance owns changes to documentation authority, hierarchy,
  knowledge promotion, or current/historical classification. Existing approval
  for concrete changes remains valid.

These are independent capabilities, not required development phases.
