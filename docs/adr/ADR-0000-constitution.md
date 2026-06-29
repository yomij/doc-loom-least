---
title: Doc Loom Constitution
type: constitution
status: active
created: 2026-06-18
updated: 2026-06-29
tags:
  - doc-loom
  - constitution
  - foundational-principles
  - docs-governance
  - lifecycle-scope
---

# Constitution

This document is the constitutional authority for Doc Loom Least.

It sits above the PRD, architecture, technical design, decision log, CLI design, skill design, phase design, and reference material. Those documents may refine, implement, or explain this constitution, but they must not weaken or contradict it.

If another Doc Loom document conflicts with this constitution, the lower-authority document must be revised, superseded, or blocked for explicit owner decision. A conflict must not be silently resolved by adding more process.

## Scope Interpretation

Doc Loom Least is the current development-flow slice of a broader personal product workflow substrate. Future product, research, design, release, or operations flows may be added only when a real workflow boundary exists and the smallest useful skill or document contract is clear.

In this constitution, "platform" may mean a repo-native, skill-based, Markdown-first place to carry a personal workflow across a product lifecycle. It must not mean a CLI backend, daemon, centralized orchestration service, or complex pipeline product.

## Constitutional Principles

### 1. Enter Through The Minimum Path

Doc Loom must solve problems through the smallest path that reaches the root issue.

Prefer the narrowest contract, artifact, checker, or skill that resolves the actual governance failure. Avoid broad migrations, duplicate artifacts, unnecessary intermediate states, and ceremony that exists only to make the system look complete.

The design standard is direct, elegant, and beautiful: fewer moving parts, clearer boundaries, stronger claims.

### 2. Do Not Become A Complex Pipeline Product

Doc Loom is not a complex pipeline product.

Expanding beyond development must happen as narrow lifecycle domains and skills, not as a broad orchestration layer. Do not add empty stages, placeholder rituals, or speculative domain machinery for future completeness.

### 3. Human-Semantic Design

Human-facing designs, documents, workflows, prompts, guides, artifacts, and explanations must be highly semantic, readable, understandable, and beautiful. They should make the domain meaning obvious, reduce cognitive overhead, and preserve aesthetic clarity through structure, naming, language, and composition.

## Non-Negotiable Consequences

- Do not add workflow stages only to make reasoning feel safer.
- Do not use a complex pipeline to solve a narrow governance failure.
- Do not treat unbound Markdown prose as official evidence.
- Do not make Doc Loom larger than the change it governs.
- Do not add lifecycle domains before a real workflow needs them.

## Amendment Rule

Changing this constitution requires an explicit owner decision. Any amendment must update this document, the documentation index, the SSOT map, and the decision log entry that records the constitutional change.
