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
