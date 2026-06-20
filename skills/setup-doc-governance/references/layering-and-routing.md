# Layering And Routing

## Documentation Layers

| Layer | Name | Default location | Role |
|---|---|---|---|
| L1 | Authority | `docs/authority/` | Confirmed reusable current facts that constrain later agents. |
| L2 | Case / Operational | `docs/cases/<case-id>/` | Current task process, plan, evidence, execution, closure. |
| L3 | Derived / Index | `docs/README.md`, `docs/DOC_INDEX.md`, README, guides | Entry points, navigation, derived explanations. Not source facts. |
| L4 | Archive / Historical | `docs/archive/` | Historical, superseded, or replaced material. Not current by default. |
| L5 | Scratch | `docs/scratch/` or case notes | Drafts, temporary notes, unverified inference. |

SSOT is a principle, not a directory. Different facts may have different source
owners: user confirmation, code, tests, ADR, external systems, or generated
views. Frontmatter must declare the primary source of truth.

## Authority Sections

Create sections only when they contain confirmed facts.

| Knowledge type | Target |
|---|---|
| Product goals, users, scope, non-goals, acceptance criteria | `docs/authority/product/**` |
| Domain concepts, glossary, invariants, state rules | `docs/authority/domain/**` |
| Architecture shape, module boundaries, flows, integrations | `docs/authority/architecture/**` |
| Architecture decisions and consequences | `docs/authority/architecture/decisions/**` |
| CLI, API, schema, config contracts | `docs/authority/contracts/**` |
| Case lifecycle, governance process, review and confirmation gates | `docs/authority/workflow/**` |
| Agent policy, context loading, adapter source | `docs/authority/agent/**` |
| Setup, release, runbooks, maintenance rules | `docs/authority/operations/**` |

`docs/authority/README.md` is routing and priority guidance. It should not hold
large amounts of domain fact.

## Index Rules

After applying governance decisions, update the docs entry index:

- Prefer existing `docs/README.md`.
- Create `docs/DOC_INDEX.md` only when no docs entry exists.
- Link to authority, governance plan, case evidence if present, archive if
  present, and derived/generated view notes.
- State that authority is the current fact entry.
- State that evidence/archive are historical or evidentiary.
- State that derived/generated views are not source of truth.

## Bridge Rules

Do not create bridges by default. Prefer a bridge only when the old path is a
common entry point or would likely mislead a future human or agent.

Bridge files are thin:

```md
# Superseded

This document is no longer the authority source.

Current authority: <target>
Historical source: <archive target>
```

Bridge files do not preserve or expand old facts.

## Archive Rules

Suggested archive layout:

```text
docs/archive/
  historical/
  superseded/
  raw/
```

Archive material should be traceable through `superseded_by` metadata or an
archive index when useful. It is evidence, not current context by default.

## Agent Instruction Governance

AI instructions belong to executable AI context. Prefer:

```text
canonical policy once -> adapters everywhere
```

Default authority hosts:

```text
docs/authority/agent/policy.md
docs/authority/agent/adapters.md
```

Adapter files such as `AGENTS.md`, `CLAUDE.md`, or editor-specific instruction
files are derived views unless the project explicitly defines otherwise.
