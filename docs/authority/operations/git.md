---
status: active
authority: true
layer: authority
type: operations
source_of_truth: user_confirmed
supersedes: []
superseded_by: []
owner: user
last_verified: 2026-07-12
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

When an approved plan declares atomic commits, each commit represents one
coherent, verified, independently reviewable and revertible semantic completion
point. It excludes unrelated changes and leaves relevant checks passing. Shared
protocol defines completion points and authorization; stage Skills define
procedure.

Case commits use deterministic trailers:

```text
Doc-Loom-Case: <case-id>
Doc-Loom-Step: requirements | plan | task:<id> | refactor:<id> | review-fix:<id> | closure
```

Actual task, refactor, and review-fix hashes are recorded in `execution.md`.
`closure.md` must not predict its own commit hash. Unqualified `Done` requires a
successful closure commit and no unexplained case-related working-tree changes.

Fast-Path is the compact exception: it creates no separate plan or task commit.
After its green delta and compact review pass, one `Doc-Loom-Step: closure`
commit may contain the approved minimal plan, implementation/verification,
`closure.md` with final status, and necessary dashboard sync. Its title describes the
user-visible change, not closure bookkeeping.

Before plan confirmation, the human-facing summary states the expected number
and purpose of local commits and makes clear that push, PR, merge, release, and
history rewriting are excluded unless separately authorized.

Plan approval does not authorize publication or history rewriting. When the
user separately authorizes a rewrite, affected hashes and review evidence are
stale and must be refreshed before closure.
