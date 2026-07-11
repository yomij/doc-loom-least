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

Skillshare discovers canonical skills recursively from this tree. `.skillignore` excludes `docs/archive/**`, and archived reference skills are no longer kept under `docs/archive/raw/reference/`.
