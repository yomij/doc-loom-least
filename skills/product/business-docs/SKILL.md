---
name: business-docs
description: Create or update business-facing documentation that captures rules, decisions, outcomes, or historical context from conversations, tasks, documents, or code. Use for standalone business-document requests or when development work produces business conclusions to retain.
---

# Business Docs

Use the user's language for all output, including templates, unless another language is requested.

Works independently of docloom-workflow and task.md. Use the requested audience;
default to general business readers.

## Audience and expression

Make the body understandable without project or technical knowledge. Explain
the business problem, actors, conditions, actions, outcomes, and exceptions;
define necessary terms on first use. Organize by business activity, preserving
exact limits and uncertainty. Explain technical constraints by their business
effects; put implementation details and technical identifiers in the source
appendix. Label illustrative examples and add no unsupported rules.

## Sources and scope

Stay within the requested topic and accessible evidence.

- **Conversation or task:** Capture requests, corrections, decisions, and stated
  reasons from visible records. Persist meaningful changes in a draft during
  work; missing history stays a source gap.
- **Historical logic:** Use relevant documents, code, tests, configuration, and
  history. Without a requested period, use the current checkout and state its
  revision/local changes. Date/version evolving rules. Source inspection does
  not prove deployment; test definitions do not prove tests passed.
- **Existing document:** Reuse the matching draft; preserve established rules
  and sources while applying supported changes.

Follow repository authority and owner decisions for intended rules. Separate
those rules from observed behavior and unresolved questions; expose conflicts.
Code cannot establish business rationale, nor agent suggestions owner approval.
Keep confirmation, delivery, and verification distinct.

Link material rules and decisions to sources. Without stable conversation links,
retain speaker, request/correction, and context as a labeled excerpt/summary in
the source appendix or task record, not a verified transcript. State missing
evidence or rationale; ask only when ambiguity changes the conclusion and
continue supported work.

## Write and retain

Read [templates/business.md](templates/business.md) for new or restructured
documents. Adapt sections to the purpose; omit empty sections, irrelevant
delivery tracking, and decision history that explains no meaningful choice.

Respect an explicit output target and repository conventions. Otherwise:

- Existing task directory: `docs/cases/<task-id>/business.md`.
- Otherwise: `docs/business/archives/<YYYY-MM-DD>-<topic>.md`; do not create
  task.md solely for this document. Use meaningful suffixes for collisions.
- No writable workspace or text-only request: return inline without claiming a save.

Update the same draft during work. State draft/archive status explicitly,
regardless of directory. Archive a completed discussion snapshot, task result,
or investigation with its date and basis; retain unresolved items. Archival
proves neither rule approval, delivery, verification, nor authority.

Preserve archived bodies. Later changes get a new document and two-way links
identifying affected rules, not whole-topic supersession. Explicit corrections
retain a dated note and source. Never overwrite an archive on a path collision.

Update existing business navigation, otherwise `docs/business/README.md`, with
topic, link, date, status, and rule relationships. The index is navigation, not a
current rulebook. Update existing authority within its ownership and authorization;
route new authority or unresolved binding-rule changes to setup-doc-governance
if available, otherwise surface the decision. Ordinary writing needs no such dependency.

## Finish

Check nontechnical readability, source/link validity, time scope, visible gaps,
and that discarded requirements are not final rules. Report location, scope,
and material uncertainties. Technical-only closure needs no empty archive;
an explicit historical summary still warrants a document without business changes.
