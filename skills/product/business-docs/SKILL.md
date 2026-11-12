---
name: business-docs
description: Draft, update, or archive business documentation that explains rules, decisions, outcomes, and historical logic in plain language for non-technical or specified readers. Use for standalone conversation/history summaries or when development work produces business conclusions to retain.
---

# Business Docs

Use the user's language for all output, including templates, unless another language is requested.

Operate independently of docloom-workflow and task.md. Write for the requested
audience, or for general non-technical business readers by default.

## Audience and expression

Make the body understandable without project or technical knowledge. Explain
the business problem, actors, conditions, actions, outcomes, and exceptions;
define necessary terms on first use. Organize by business activity, preserving
exact limits and uncertainty. Explain technical constraints by their business
effects in the body; put implementation details and technical identifiers in the
source appendix. Label illustrative examples and add no unsupported rules.

## Sources and scope

Stay within the requested topic and accessible evidence:

- **Conversation or task:** Capture requests, corrections, decisions, and stated
  reasons from visible records. Update the draft as meaningful changes occur
  during work; record missing history as a source gap.
- **Historical logic:** Reconstruct rules from relevant documents, code, tests,
  configuration, and version history. Without a requested period, use the
  current checkout and state its revision/local changes. Attach dates or
  versions when rules evolve. Source inspection does not prove deployment; test
  definitions do not prove tests passed.
- **Existing document:** Update the matching draft in place, preserving
  established rules and sources while applying supported changes.

Follow repository authority and owner decisions for intended rules. Separate
intended rules, observed behavior, and open questions; expose conflicts. Do not
infer business rationale from code alone, or treat unconfirmed agent suggestions
as approved rules. Keep confirmation, delivery, and verification distinct.

Link material rules and decisions to sources. Without stable conversation links,
retain speaker, request/correction, and context as a labeled excerpt/summary in
the source appendix or task record, not a verified transcript. State missing
evidence or rationale; ask only when ambiguity changes the conclusion, and
continue supported work.

## Write and retain

Read [templates/business.md](templates/business.md) **conditionally**, when
creating or restructuring a document. Adapt sections to the purpose; omit empty
sections, irrelevant delivery tracking, and decision history that explains no
meaningful choice.

- **Output target:** Respect an explicit target path and repository conventions.
  Otherwise use `docs/cases/<task-id>/business.md` for an existing task
  directory, or `docs/business/archives/<YYYY-MM-DD>-<topic>.md` without
  creating task.md solely for this document (using meaningful suffixes for
  collisions). With no writable workspace or for a text-only request, return
  inline without claiming a save.
- **Draft and archive lifecycle:** Update the same draft during work and state
  draft/archive status explicitly regardless of directory. Archive a completed
  discussion snapshot, task result, or investigation with its date and basis;
  retain unresolved items. Archival proves neither rule approval, delivery,
  verification, nor authority.
- **Preserving history:** Preserve archived bodies and never overwrite an
  archive on a path collision. Record later changes in a new document with
  two-way links identifying affected rules, not whole-topic supersession.
  Explicit corrections retain a dated note and source.
- **Navigation and authority:** Update the existing business index, or
  `docs/business/README.md`, with topic, link, date, status, and rule
  relationships. The index is navigation, not a current rulebook. Update
  existing authority within its ownership and authorization; route new authority
  or unresolved binding-rule changes to setup-doc-governance if available,
  otherwise surface the decision. Ordinary writing needs no governance dependency.

## Finish

Check nontechnical readability, source/link validity, time scope, visible gaps,
and that discarded requirements are not final rules. Report location, scope,
and material uncertainties. Technical-only closure needs no empty archive; an
explicit historical summary still warrants a document without business changes.
