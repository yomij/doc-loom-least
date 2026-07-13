# Skills Layout

Doc Loom Least skills are grouped by lifecycle domain and cross-cutting capability. The grouping is physical organization only; each skill keeps its frontmatter `name`, so invocation names stay stable.

| Directory | Contains | Rule |
|---|---|---|
| `development/` | `docloom-workflow`, `context-authority`, `plan-confirm`, `tdd-execute`, `doc-sync-close` | Current primary development flow. |
| `governance/` | `setup-doc-governance` | Documentation and knowledge governance. |
| `assessment/` | `review`, `grill` | Read-only ad-hoc/workflow review and manual challenge helpers, available across lifecycle domains. |
| `_shared/` | Shared protocol and templates | Not a directly invoked skill. |

`docloom-workflow` is the recommended human-facing doorway. Stage skill names
remain stable for explicit expert use and internal ownership, but ordinary
persistent development requests should route without requiring the user to
select a stage.

Future domains such as `product/`, `research/`, and `design/` should be added only when real skills exist. Do not add empty directories as roadmap placeholders.

## Loading Discipline

- Keep frontmatter triggers exact and distinguish the public doorway from
  explicit/internal stage owners.
- Keep cross-skill invariants in `_shared`; keep complete procedure at one
  semantic owner.
- Route or invoke another Skill by its stable frontmatter `name`, never by
  importing its `SKILL.md` or private filesystem path.
- Put a multi-owner file contract in `_shared` and expose a readable local path
  to each consumer. Treat other local `references/` and `templates/` as private.
- Allow intentional workflow return routes, but keep physical file dependencies
  free of cross-Skill edges and cycles.
- Load references/templates only for their named condition; do not move prose
  from one mandatory path to another.
- Recalculate unique required files, not only total lines, after a workflow
  contract change.

Current verified word-cost ceilings:

| Path | Ceiling |
|---|---:|
| Default doorway | 1,700 |
| Context through plan | 3,700 |
| Typical full persistent flow | 7,200 |
| All active `skills/**` Markdown | 11,100 |

Skillshare discovers canonical skills recursively from this tree. `.skillignore` excludes `docs/archive/**`, and archived reference skills are no longer kept under `docs/archive/raw/reference/`.
