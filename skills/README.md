# Skills Layout

Doc Loom Least skills are grouped by lifecycle domain and cross-cutting capability. The grouping is physical organization only; each skill keeps its frontmatter `name`, so invocation names stay stable.

| Directory | Contains | Rule |
|---|---|---|
| `development/` | `docloom-workflow`, `context-authority`, `plan-confirm`, `tdd-execute`, `doc-sync-close` | Current primary development flow. |
| `governance/` | `setup-doc-governance` | Documentation and knowledge governance. |
| `assessment/` | `review`, `grill` | Manual review and challenge helpers, available across lifecycle domains. |
| `_shared/` | Shared protocol and templates | Not a directly invoked skill. |

Future domains such as `product/`, `research/`, and `design/` should be added only when real skills exist. Do not add empty directories as roadmap placeholders.

Skillshare discovers canonical skills recursively from this tree. `.skillignore` excludes historical reference skills under `docs/reference/skills/`.
