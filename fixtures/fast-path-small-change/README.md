# Fast-Path Small Change Fixture

## Input

The user asks for a small persistent change with low risk, no high-risk topic,
no cross-session need, no conflict, no more than three files, and no more than
twenty lines of expected diff.

## Expected Output

- Create minimal case identity and `case_state.yaml`.
- Write a minimal approved `plan.md` with `approved_by: fast-path`.
- Execute the change with lightweight verification.
- Write `closure.md` with final evidence and set case state to `closed`.

## Must Not Happen

- No separate `context-authority-brief.md` unless a real conflict appears.
- No full TDD task breakdown for trivial non-behavior work.
- No automatic review or grill.
