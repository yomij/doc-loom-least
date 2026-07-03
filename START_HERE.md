# Start Here

Use the smallest path that handles the work.

## Pick A Path

| Work | Use | Create |
|---|---|---|
| One-off low-risk docs or copy edit | Edit directly after checking authority | No case docs |
| Small persistent change | Fast-Path case | `case_state.yaml`, minimal `plan.md`, `closure.md` |
| Risky, cross-session, or behavior-changing work | Full case workflow | `plan.md`, optional `context-authority-brief.md`, `execution.md`, `closure.md` |

If the user only asks for status, do not create a case. Report the current
phase, evidence, next skill, and missing input.

## User-Facing States

Humans only need four states:

| State | Meaning |
|---|---|
| `status-only` | Read and report; no files changed. |
| `planned` | A plan exists and execution still needs approval or a valid Fast-Path record. |
| `executing` | An approved plan is being implemented and checked. |
| `closed` | Closure records final status, evidence, caveats, and follow-ups. |

`case_state.yaml` may contain narrower routing phases such as
`waiting_for_plan_confirmation` or `doc_syncing`. Treat those as agent routing
details, not extra concepts the user must manage.

## Try It Without Installing

Read `examples/minimal-project/README.md` for a tiny repo-shaped example.

Run local checks from this repository:

```bash
scripts/verify
scripts/verify-fixtures
```

## Full Rules

- Product and authority: `docs/authority/README.md`
- Development workflow: `docs/authority/workflow/development-flow.md`
- Full shared protocol: `skills/_shared/references/shared-protocol.md`
- Installation: `INSTALL.md`
