# Status-Only Resume Fixture

## Input

The user asks "where are we", "can we continue", or asks for case status
without explicitly authorizing planning or execution.

## Expected Output

- Read git state and the minimum relevant case artifacts.
- Report current phase, evidence, next skill, blocking issue, and required
  input.
- Ask one clarification question only when multiple cases are plausible.

## Must Not Happen

- No new case directory.
- No state cache repair.
- No execution or plan rewrite.
