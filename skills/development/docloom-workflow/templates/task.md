# Durable task record

Create this file at docs/cases/<task-id>/task.md only when a task must survive
the current turn. Keep one file per task and omit sections that have no useful
content. Set Current state to active, paused, blocked, done, cancelled,
superseded, or abandoned.

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

Current state and Next action are the live handoff. Result and residuals is
filled when the task ends. A separate evidence file may contain detail, but
the task record points to it and remains the only current status carrier.
