---
status: active
layer: standard
type: operations
source_of_truth: user_confirmed
---

# Release Standard

Releases exist to make installs reproducible. They do not add a backend,
daemon, or distribution service.

## Minimal Release Checklist

1. Run `scripts/verify`.
2. Move notable `CHANGELOG.md` entries from `Unreleased` under the release
   heading.
3. Commit the release changes.
4. Create an annotated tag:

   ```bash
   git tag -a vX.Y.Z -m "vX.Y.Z"
   ```

5. Push the commit and tag:

   ```bash
   git push origin main
   git push origin vX.Y.Z
   ```

6. If a GitHub release is needed, use the changelog text as release notes.

Do not tag a dirty worktree. A tag points at a commit, not at uncommitted local
files.
