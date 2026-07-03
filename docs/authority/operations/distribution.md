---
status: active
authority: true
layer: authority
type: operations
source_of_truth: code
supersedes: []
superseded_by: []
last_verified: 2026-07-03
---

# Distribution

Doc Loom Least is distributed as Agent Skills. The supported install path is
`skillshare`.

Expected canonical skill frontmatter names:

- `docloom-workflow`
- `setup-doc-governance`
- `context-authority`
- `plan-confirm`
- `tdd-execute`
- `doc-sync-close`
- `review`
- `grill`

Canonical skill discovery is limited to the grouped `skills/` tree. Archived
reference skills are no longer kept under `docs/archive/raw/reference/`.
`.skillignore` excludes `docs/archive/**`, so archived or imported evidence is
not published as canonical skills if it contains skill files.

Shared protocol files are exposed into per-skill references through symlinks to
`skills/_shared/references/shared-protocol.md`. Shared handoff templates are
exposed through symlinks to `skills/_shared/templates/handoff.md`.

## Local Verification

Maintainers can run static repository checks without adding a runtime backend:

```bash
scripts/verify
scripts/verify-fixtures
```

These scripts verify canonical skill names, authority frontmatter, case-state
shape, shared symlinks, fixture structure, and whitespace checks. They are
maintenance checks, not a product CLI.

## Release Reproducibility

Release tags are used only to make installs reproducible. Tags must point at
clean commits, and release notes should come from `CHANGELOG.md`. The release
checklist lives in `docs/standards/release.md`.

## Sources

- [INSTALL.md](../../../INSTALL.md)
- [Release Standard](../../../docs/standards/release.md)
- [.skillignore](../../../.skillignore)
- Current symlinks under `skills/**/references/shared-protocol.md`
- Current symlinks under `skills/**/templates/handoff.md`
