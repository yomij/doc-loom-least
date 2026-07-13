# Governance Rules

## Documentation Layers

| Layer | Name | Default location | Role |
|---|---|---|---|
| L1 | Authority | `docs/authority/` | Confirmed reusable current facts that constrain later agents. |
| L2 | Case / Operational | `docs/cases/<case-id>/` | Current task process, plan, evidence, execution, closure. |
| L3 | Derived / Index | `docs/README.md`, README, guides | Entry points, navigation, derived explanations. Not source facts. |
| L4 | Archive / Historical | `docs/archive/` | Historical, superseded, or replaced material. Not current by default. |
| L5 | Scratch | `docs/scratch/` or case notes | Drafts, temporary notes, unverified inference. |

SSOT is a principle, not a directory. Different facts may have different source
owners: user confirmation, code, tests, ADR, external systems, or generated
views. Frontmatter must declare the primary source of truth.

## Authority Sections

Create sections only when they contain confirmed facts.

| Knowledge type | Target |
|---|---|
| Project constitution: principle-level non-negotiable clauses | `docs/authority/constitution.md` |
| Product goals, users, scope, non-goals, acceptance criteria | `docs/authority/product/**` |
| Domain concepts, glossary, invariants, state rules | `docs/authority/domain/**` |
| Architecture shape, module boundaries, flows, integrations | `docs/authority/architecture/**` |
| Architecture decisions and consequences | `docs/authority/architecture/decisions/**` |
| CLI, API, schema, config contracts | `docs/authority/contracts/**` |
| Case lifecycle, governance process, review and confirmation gates | `docs/authority/workflow/**` |
| Agent policy, context loading, adapter source | `docs/authority/agent/**` |
| Setup, release, runbooks, maintenance rules | `docs/authority/operations/**` |

Read the declared active constitution first. `docs/authority/README.md` is
routing and priority guidance, not a large domain fact host.

## Routing Verdicts

Use one verdict set for files and facts:

| Verdict | File or fact handling |
|---|---|
| `promote` | Extract a current fact into a new authority doc. |
| `merge` | Merge a current fact into an existing authority doc. |
| `bridge` | Keep only a thin old-path entry pointing to current authority or archive. |
| `archive` | Preserve historical value outside current-fact context. |
| `block` | Conflict, high risk, or insufficient evidence requires owner decision. |

Do not introduce `candidate`, `accepted`, `canonical`, or `deprecated` as a
default state matrix.

Code and tests may be read as evidence in `full-repo` scope and are never
modified. They may override historical docs for low-risk internal details only
when the topic is not high risk, the historical source is not active authority,
and code and tests agree. Use `block` for high-risk topics, ADR boundaries,
recent owner-confirmed facts, code/test disagreement, or missing high-impact
coverage.

## Index And Entry Points

After applying governance decisions, update the docs entry index:

- Prefer existing `docs/README.md`.
- Link authority, governance plan, case evidence, archive, and derived/generated
  view notes only when present.
- State that authority is the current fact entry, evidence/archive are
  historical or evidentiary, and derived/generated views are not source facts.

Use the narrowest entry: root `README.md` for the repo, `docs/README.md` for area
routes and important docs, and local `README.md` only for top-level layers or
complex domains. Agent files such as `AGENTS.md`, `CLAUDE.md`, or editor-specific
instructions receive a thin derived pointer. Plain directories record `none`.
Do not list every leaf doc at the top level. Entry authority and conflict chains
must match the shared fact-authority order.

## Bridge And Archive Rules

Do not create bridges by default. Use one only when an old common entry would
otherwise mislead a future human or agent. A bridge is thin:

```md
# Superseded

This document is no longer the authority source.

Current authority: <target>
Historical source: <archive target>
```

Bridges do not preserve or expand old facts.

Suggested archive layout is `docs/archive/historical/`,
`docs/archive/superseded/`, and `docs/archive/raw/`. Archive material should be
traceable through `superseded_by` metadata or an archive index when useful. Any
document under `archive/`, `old/`, or `deprecated/`, or marked deprecated, must
carry `status: archived | superseded`; it is not current fact by default.

A document without an active status is not current fact and must not enter
current-fact context.
