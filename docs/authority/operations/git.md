---
status: active
authority: true
layer: authority
type: operations
source_of_truth: user_confirmed
supersedes: []
superseded_by: []
last_verified: 2026-07-02
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

## Semantic Atomic Commits

When an approved Doc Loom plan declares an Atomic Commit Strategy, approval
authorizes only the case-scoped commits listed in that strategy. Normal semantic
completion points are approved requirements when present, approved plan,
independently valid green task, independently verified refactor, material
review-fix root cause, and closure.

Atomic means one coherent intent that:

- contains no unrelated user or other-case changes;
- includes the implementation, tests or alternative verification, and
  necessary task-local docs for that intent;
- leaves the relevant repository checks passing;
- can be reviewed and reverted independently.

Do not create normal completion commits for TDD Red states, failed attempts,
empty stages, checkbox-only progress, or timestamps. If two planned tasks
cannot be separated without leaving an invalid repository, combine the
smallest dependent unit.

Before every authorized commit, stage explicit paths, inspect the staged diff,
run `git diff --cached --check`, and verify the relevant checks. Do not use
broad staging when unrelated changes may exist. If a mixed file cannot be
isolated safely, stop instead of committing it.

Case commits should carry deterministic trailers:

```text
Doc-Loom-Case: <case-id>
Doc-Loom-Step: requirements | plan | task:<id> | refactor:<id> | review-fix:<id> | closure
```

Actual task, refactor, and review-fix hashes are recorded in `execution.md`.
`closure.md` must not predict its own commit hash. Unqualified `Done` requires a
successful closure commit and no unexplained case-related working-tree changes.

Plan approval does not authorize push, PR, merge, tag, release, amend, rebase,
squash, history rewriting, unrelated files, material plan deviations, or
unlisted dependency, lockfile, CI, schema, config-contract, or authority work.

When the user separately authorizes history rewriting, previously recorded
affected hashes and review evidence become stale. Revalidate the exact baseline
and rewritten range, refresh execution evidence, and repeat the final
Engineering/Spec review before closure.
