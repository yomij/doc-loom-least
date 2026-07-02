# Document Update Rules

## Layer Rules

| Layer | May update automatically | Rule |
|---|---:|---|
| L1 Authority | Conditional | Default proposal. Only confirmed narrow patch may be applied. |
| L2 Operational | Yes | Update task process docs. |
| L3 Derived | Conditional | Mechanical derived sync only. |
| L4 Historical | No | Do not change by default. |
| L5 Scratch | Conditional | May clean or promote only with source handling. |

## Narrow Authority Patch

All conditions must hold:

- Target authority doc already exists.
- Patch synchronizes a small part of an existing authority fact.
- Evidence comes from this case plan, execution, tests, diff, or explicit user
  confirmation.
- No new authority area is created.
- No file movement, archive, or bridge is involved.
- No conflict fact, unresolved High-Risk Topic decision, or lifecycle migration
  is involved.
- User confirms the concrete patch per shared-protocol → Confirmation Semantics.

If any condition fails, write an authority proposal or governance follow-up
instead.

## Mechanical L3 Sync

Allowed when source is traceable, no long-term fact changes, no authority
conflict:

- README links, paths, and status entries.
- Troubleshooting with a verified reproducible short entry.
- Examples with commands, paths, or parameters changed by code.
- Getting-started/dev-guide references to confirmed contracts.

Proposal only:

- Product goal changes.
- Architecture principle changes.
- Workflow policy.
- Best-practice generalization from one case.
- Tutorial rewrite requiring owner judgment.

## Authority Change Table

Use this closure table:

```md
| Doc | Status | Proposed / Applied Change | Evidence | Risk | Next |
|---|---|---|---|---|---|
```

Rules:

- `Status`: `proposed` or `applied`.
- `Risk`: `low`, `medium`, or `high`.
- `Next`: `none`, `confirm_narrow_patch`, `governance_plan`, or `new_case`.
- High-risk authority proposals must mention `owner` and `last_verified`
  requirements.
- Proposals from `plan.md ## Decisions` must explain why a task-scoped
  decision should become long-term authority.

## Lifecycle Status

Authority status values are only:

- `active`
- `draft`
- `superseded`
- `archived`

`in_review` and `needs_refresh` are proposal reasons, not frontmatter status
values. High-impact docs must not be marked `active` by the agent alone.
