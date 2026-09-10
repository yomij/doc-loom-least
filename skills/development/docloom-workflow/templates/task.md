# Durable task record

Read when creating or restructuring a record at docs/cases/<task-id>/task.md.
Use for continuity, a reusable decision, an interrupted handoff, or an explicit
case request. Omit unused sections; routine updates need no template reload.

~~~md
# Task

## Goal

## Success conditions

## Constraints and decisions

## Current state

## Next action

## Verification evidence

## Result and residuals
~~~

Current state is active, paused, blocked, done, cancelled, superseded, or
abandoned. State the evidence and blocker if present. This record owns status
and the next action; attachments provide evidence only. Mark done only after
the Skill's verification conditions hold.
