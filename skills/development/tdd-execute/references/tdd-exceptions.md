# TDD Exceptions

TDD exceptions are allowed only when confirmed in `plan.md` or a confirmed
same-turn amendment. An exception never means "skip verification".

Allowed categories:

- Pure documentation change.
- Configuration adjustment.
- Build script repair.
- Logging or telemetry.
- UI copy.
- Dead code deletion.
- Exploratory spike.
- Urgent hotfix.
- Environment change that cannot be reliably automated.

Required record:

```md
## TDD Applicability

- TDD Required: No
- If No, Reason:
- Alternative Verification:
  - manual check
  - snapshot check
  - build check
  - smoke test
  - reviewer confirmation
```

Manual verification must record steps, environment, result, and limitation.

Behavior-preserving refactors should use:

```text
characterize existing behavior -> refactor -> verify no regression
```

Do not invent a meaningless failing test just to satisfy Red.
