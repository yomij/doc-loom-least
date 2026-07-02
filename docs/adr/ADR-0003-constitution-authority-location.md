---
title: Constitution Authority Location
type: adr
status: accepted
created: 2026-07-02
updated: 2026-07-02
tags:
  - doc-loom
  - constitution
  - docs-governance
  - authority
---

# ADR-0003: Constitution Authority Location

## Context

The constitution originally lived at `docs/adr/ADR-0000-constitution.md`. After
the authority layer was introduced, that left the highest-priority
constitutional text outside the governed authority directory.

## Decision

The active constitution now lives at `docs/authority/constitution.md`.
`docs/adr/ADR-0000-constitution.md` was removed; use the active constitution
path directly.

Future constitutional amendments must update the active constitution, the
documentation index, the SSOT map, and the decision log.

## Consequences

- Agents can discover the constitution through `docs/authority/README.md`.
- ADRs remain decision records, not the active constitutional text.
- Adapter files should point to the active constitution and authority index.
