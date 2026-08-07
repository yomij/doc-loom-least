---
status: active
authority: true
layer: authority
type: operations
source_of_truth: user_confirmed
supersedes: []
superseded_by: []
owner: user
last_verified: 2026-08-07
---

# Git Commit Titles

Except for Git auto-generated messages such as `Merge ...` and `Revert ...`,
all commit titles must use:

```text
<type>: <summary>
```

`type` is required, lowercase, and limited to:

- `feat`
- `fix`
- `docs`
- `chore`
- `refactor`
- `style`
- `test`
- `perf`
- `revert`

`summary` is required and should briefly describe the business object and
action. Use a single English colon between `type` and `summary`.

## Source

- [Git Commit Standard](../../standards/git-commit-standard.md)

## Case Commit Policy

When user/project policy or an approved Constraint requires or permits commits,
each commit represents one coherent, verified, independently reviewable and
revertible semantic completion point. It excludes unrelated changes and leaves
relevant checks passing. The plan body never carries commit strategy;
`tdd-execute` chooses organization inside the authorized outcome contract.
Shared protocol defines completion points and authorization; stage Skills
define procedure.

Case commits use deterministic trailers:

```text
Doc-Loom-Case: <case-id>
Doc-Loom-Step: requirements | plan | plan-amendment | task:<id> | refactor:<id> | review-fix:<id> | closure
```

Use `plan-amendment` only for a materially changed plan version that was
reconfirmed after the original approved plan commit.

Actual task, refactor, and review-fix hashes are recorded in `execution.md` when
that artifact exists. Terminal carriers (`closure.md` or Compact plan close
fields) must not predict their own completion commit hash.

Local commits follow user/project intent, approved Constraints, and semantic
value. Ordinary case bookkeeping does not require standalone plan or closure
commits. Review findings may be fixed in coherent batches; finding count does
not dictate commit count. Unqualified `Done` requires commit success only when
user/project policy or an approved Constraint explicitly requires it, plus no
unexplained case-related working-tree changes.

Before guarded plan confirmation, the human-facing summary states expected
local Git actions and makes clear that push, PR, merge, release, and history
rewriting are excluded unless separately authorized.

Plan approval does not authorize publication or history rewriting. When the
user separately authorizes a rewrite, affected hashes and review evidence are
stale and must be refreshed before closure.
