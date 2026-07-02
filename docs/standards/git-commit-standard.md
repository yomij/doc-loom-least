# Git Commit Standards

Except for Git auto-generated messages like `Merge ...` and `Revert ...`, all commit titles **must** use the format:

```text
<type>: <summary>
```

- `type` is **required**, lowercase only, and limited to:
  `feat`, `fix`, `docs`, `chore`, `refactor`, `style`, `test`, `perf`, `revert`.
- `summary` is **required**, briefly describing the business object and action.
- A single English colon `:` **must** be placed between `type` and `summary`.

Example:

```text
feat: add xxx feature
```