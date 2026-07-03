# Full Case Dev Task Fixture

## Input

The user asks for behavior-changing, medium-risk, high-risk, cross-session, or
public-contract work.

## Expected Output

- Resolve or create a case id through `docloom-workflow`.
- Run `context-authority` when authority, resume, conflict, or high-risk
  conditions apply.
- Write a draft `plan.md` and wait for user approval.
- Execute only after approval, with TDD or a recorded exception.
- Close with `closure.md` and acceptance evidence.

## Must Not Happen

- No implementation before the current plan is approved.
- No unqualified `Done` with unmet or unverified acceptance criteria.
- No authority patch without a concrete confirmed narrow change.
