---
name: docloom-workflow
description: Execute repository development work from planning and implementation through verification, including task resumption and ordinary documentation edits. Use when asked to build, fix, refactor, plan, resume, or update project files; excludes standalone review, assumption-challenging dialogue, business-rule documentation, and structural doc governance.
---

# Doc Loom Workflow

Use the user's language for all output, including templates, unless another language is requested.

Work directly toward the user's outcome; do not ask the user to choose a phase.

## Work

- Establish the outcome, success conditions, and constraints from the request,
  repository instructions, worktree, and evidence. Discovery- or planning-only
  requests produce an evidence-based recommendation, tradeoff, and uncertainty;
  they do not authorize implementation.
- Follow declared fact ownership: active owner-approved rules constrain work,
  while implementation shows behavior. Cases, derived views, history, and
  scratch do not establish authority. Do not elevate implementation preferences
  into requirements.
- Resolve conflicts using declared precedence and existing owner decisions.
  Ask only when an unresolved conflict or missing fact changes the outcome,
  constraints, public contract, permissions, or durable authority. Continue
  independent work while that decision is pending.
- Execute within existing authorization, which covers choosing files, tools,
  sequence, tests, and necessary dependency/lockfile changes. Reuse existing
  approval; vague requests do not authorize unspecified destructive targets.
  Before an unauthorized destructive, irreversible, permission, data-deletion,
  publication, or public-contract change, make its target, scope, impact,
  recovery limits, and stopping condition concrete and obtain authorization.
- Update affected documentation within existing rules without promoting
  inferences into authority or restructuring governance as routine maintenance.
- Check each task for business content. When requirements establish or clarify
  business rules or decisions, invoke business-docs by Skill name (not a
  sibling-directory path) to capture them and meaningful corrections during
  work, then finalize and link its business document at closure. Technical-only
  work reports no business change and needs no empty archive. If business-docs
  is unavailable, retain the conclusions and source gaps in the record or
  response, disclose incomplete archival, and neither claim documentation is
  complete nor install globally.

## Verification

Check each success condition with the smallest check that demonstrates it.
Report the checks, results, and missing evidence. Repeat checks only for changed
behavior, failures, or unresolved evidence gaps.

Invoke review (a separate read-only pass requiring no additional agent) for an
explicit request, security-sensitive behavior changes (such as secret handling
or untrusted input execution), authentication or permission changes, public
API/contract changes, data migrations, or changes to shared deployment, storage,
or build behavior used by multiple applications. Ordinary bug fixes, localized
refactors, and small features outside these triggers need local verification only.

Finish when every success condition has evidence and required review has no
unresolved Critical/Important findings or conclusion-blocking gaps. Report the
outcome, residuals, and anything unverified or blocked.

## Persistence

- **When to record:** Create `docs/cases/<task-id>/task.md` only for continuity
  beyond this turn, reusable decisions that need recording, an interrupted
  handoff, or an explicit case request. Reversible one-turn work needs no record
  when none of the above conditions applies.
- **Naming and template:** Use `<YYYYMMDD>-<short-slug>` for new task IDs (the
  case creation date in the user's timezone plus a short descriptive slug);
  reuse the existing directory when resuming. Read
  [templates/task.md](templates/task.md) **conditionally**, when creating or
  restructuring that record; routine updates need no reload.
- **Status ownership:** The record owns status (active, paused, blocked, done,
  cancelled, superseded, or abandoned), evidence, any blocker, and next action;
  separate logs and reviews are evidence only. Done requires the verification
  above. Link any business document from the record without duplicating its body.
- **Resume:** Recover intent from the latest instruction and task record, then
  inspect changed worktree and relevant evidence. Ask only if ambiguity or stale
  context changes the outcome, or additional authorization is needed. Elapsed
  time alone neither expires nor extends authorization. Keep legacy
  plan/execution/closure files as evidence without migrating them by default.

## Escalation

- review owns findings; this executor owns authorized fixes and durable status.
- business-docs owns standalone business writing, task business archives, and
  historical business reconstruction; it does not require this workflow.
- grill applies only to an explicit request to challenge assumptions or claims.
- setup-doc-governance owns changes to documentation authority, hierarchy,
  knowledge promotion, and current/historical classification; existing approval
  for concrete changes remains valid.

These are independent capabilities, not sequential development phases.
