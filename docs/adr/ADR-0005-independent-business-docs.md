---
title: Independent Business Documentation
type: adr
status: accepted
created: 2026-09-10
updated: 2026-09-10
---

# ADR-0005: Independent Business Documentation

## Context

The owner needs business rules and decisions retained for product, QA, and
development without reading code or reconstructing a conversation. The request
explicitly includes sessions outside Docloom and investigation of historical
logic. A task-only closure attachment cannot cover those independent uses.

## Decision

Add business-docs under skills/product/ as a fifth discoverable Skill. It owns
one business-document contract for conversation synthesis, task archival,
historical reconstruction, and supported updates. docloom-workflow invokes it
for business conclusions and corrections during work and finalizes the document
at closure. Purely technical tasks need no empty business archive.

Standalone use requires neither docloom-workflow nor task.md. Follow existing
placement conventions, otherwise use a task's business.md or a dated document
in docs/business/archives/. Reuse drafts, preserve archived bodies, link later
changes at rule level, and maintain a thin business navigation index. Archival
records a completed snapshot, not rule confirmation or implementation success.

The narrative is business-facing. Technical material can establish observed
behavior but not invented business intent, rationale, or deployed verification.
Material conclusions retain sources and time/version scope. Authority promotion
continues to follow existing governance and owner authorization.

This decision extends ADR-0004's four-Skill install set and optional task-record
contract only for business documentation. It creates one narrow product
capability, not a full product lifecycle or new workflow phase. It applies the
constitution's existing extension rule without amending the constitution.

## Source and consequences

The owner approved execution after requesting task business archives,
independent conversation use, and historical logic reconstruction. See the
[task's source record](../cases/20260910-independent-business-docs/task.md#request-and-authorization).
Existing historical records are not migrated, and global installations are not
changed by repository work. Installed workflow use requires business-docs to
be available; incomplete archival is reported when it is absent.
