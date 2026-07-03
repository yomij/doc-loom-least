# One-Shot Doc Edit Fixture

## Input

The user asks for a typo, wording, or local documentation edit that does not
touch authority, workflow policy, public contracts, dependencies, or runtime
behavior.

## Expected Output

- Read the nearest relevant authority if needed.
- Edit only the target documentation.
- Summarize the changed file and any verification run.

## Must Not Happen

- No `docs/cases/<case-id>/` directory.
- No `plan.md`, `execution.md`, or `closure.md`.
- No authority promotion.
