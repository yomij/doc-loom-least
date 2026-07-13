---
status: active
authority: true
layer: authority
type: operations
source_of_truth: code
supersedes: []
superseded_by: []
last_verified: 2026-07-13
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

Multi-owner file contracts are stored under `skills/_shared/` and exposed to
each consumer through local relative symlinks. Current shared reference files
are `shared-protocol.md`, `loop-protocol.md`, `tdd-exceptions.md`, and
`doc-update-rules.md`; `loop-protocol.md` remains the explicit single-consumer
discovery reference. The shared handoff template is exposed only to its direct
writers, `tdd-execute` and `doc-sync-close`. Runtime companion-Skill
relationships use canonical frontmatter names and rely on the supported
repository-wide installation, not cross-Skill filesystem imports.

## Sources

- [INSTALL.md](../../../INSTALL.md)
- [.skillignore](../../../.skillignore)
- Current symlinks under `skills/**/references/*.md`
- Current writer symlinks under `skills/**/templates/handoff.md`
