---
status: active
authority: true
layer: authority
type: operations
source_of_truth: code
supersedes: []
superseded_by: []
last_verified: 2026-07-02
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

## Sources

- [INSTALL.md](../../../INSTALL.md)
- [.skillignore](../../../.skillignore)
- Current symlinks under `skills/**/references/shared-protocol.md`
- Current symlinks under `skills/**/templates/handoff.md`
