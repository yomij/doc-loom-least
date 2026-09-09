---
name: docloom-workflow
description: Manage a development task from request to verified result, including optional durable task notes, status/continuation, discovery, and human decision boundaries.
---

# Doc Loom Workflow

This is the normal development entry. Work directly toward the user's outcome;
do not make the user choose a phase. Read templates/task.md only when
the work needs a durable record, continuation, or explicit case history.

## Work

1. Read repository instructions, relevant active authority, the actual worktree,
   and current implementation evidence. Treat historical, derived, and scratch
   material as evidence rather than current fact. If current authority conflicts
   with implementation or a user-provided fact, surface the conflict before
   making a consequential choice.
2. Restate the outcome, observable success conditions, and meaningful constraints
   in working notes or conversation. Preserve a user decision when it changes a
   durable rule; do not turn an implementation preference into a requirement.
3. Execute within the user's authorization. Choose files, sequence, tools,
   tests, TDD, and commit organization yourself. A required dependency or lock
   file change is part of an authorized change when it is necessary to achieve
   the stated outcome. Stop and ask only when the outcome or constraints change,
   a material fact cannot be established, or a new consequential/irreversible
   action falls outside the authorization. For destructive or irreversible work,
   first make the exact target, scope, owner, rollback or recovery, and stopping
   condition concrete; do not perform the action on a vague instruction.
4. Verify every success condition with the smallest credible check. Record what
   was checked and its result; label missing or weak evidence as such. For an
   important compatibility, permission, governance, public-contract, or
   high-consequence change, invoke review for a read-only independent check.
5. Update only affected current or derived documentation. Do not silently turn
   an inference into authority. If a durable authority change or material
   conflict needs an owner decision, use setup-doc-governance.
6. Report the outcome, verification evidence, residual risk, and next action.
   Use Done only when all stated success conditions have credible evidence.

## Durable task record

Do not create a record for a reversible one-turn task unless the user asks for
one. When continuity, a durable decision, a meaningful interruption, or an
explicit case request makes a record useful, create
docs/cases/<task-id>/task.md and keep its current state there. Use
templates/task.md; add detail only when it helps a
future agent understand, verify, or resume the work. Large logs and reviews may
be separate evidence, but they do not own task status.

On resume, read the task record and the minimum changed authority,
implementation, dependency, and worktree evidence. Reconfirm current user
intent; time elapsed alone never expires or extends authorization. Legacy cases
with plan.md, execution.md, or closure.md remain readable as historical
evidence; do not rewrite them merely to fit this record.

## Status and discovery

Status is one of active, paused, blocked, done, cancelled, superseded, or
abandoned, with a concise evidence, blocker, and next action statement in the
task record or conversation. Do not derive a second state machine from several
files. Discovery is read-only: inspect current product facts,
follow-ups, and targeted evidence; recommend a candidate with its reason,
tradeoff, and key uncertainty. A recommendation is not execution authority.

## Explicit helpers

- review is read-only. Use it when the user asks for review or when the risk
  and evidence warrant an independent check; the executor owns corrections.
- grill is manual and conversational. Use it only when the user asks to
  challenge a claim, design, or assumption. Ask one consequential question at a
  time, verify discoverable facts, and change no files or task state.
- setup-doc-governance owns structural documentation governance and its
  confirmation gate; do not reproduce its plan machinery here.

Keep routing internal, avoid ceremony that does not protect a real decision,
and preserve the project's repo-native, Markdown-first boundary.
