---
name: business-docs
description: Create or update business documentation for product, QA, and development readers by organizing business rules and decisions. Use to distill requirements discussions, archive task business outcomes, or reconstruct current or historical business logic from documents and code. Excludes technical design and documentation authority governance.
---

# Business Docs

Produce a standalone business document for product, QA, and development readers.
Use the user's language (Chinese by default). Explain actors, conditions,
actions, outcomes, exceptions, scope, and decisions without requiring code
knowledge. Technical sources belong in the source appendix, not the business
narrative. This Skill works independently of docloom-workflow and task.md.

## Sources and scope

Infer the topic, requested time/version, and output location from the request
and existing documents. Use supplied or accessible relevant material; do not
expand a focused topic into a repository-wide investigation.

- **Conversation or task:** Extract the initial request, explicit corrections,
  decisions and their stated reasons from visible conversation and supplied
  records. During ongoing work, persist meaningful business changes in a draft;
  do not wait until closure or transcribe every exchange. Missing conversation
  history is a source gap, not permission to reconstruct what someone said.
- **Historical logic:** Use old documents, task records, relevant code, tests,
  configuration, and change history within the allowed evidence scope. Default
  to current checked-out behavior when no period is specified, state that scope
  and revision/local-change context, and distinguish it from historical rules.
  For evolution requests, date/version each change. A test file describes an
  expectation; only an observed test result is verification evidence. Reading
  source does not establish deployed behavior.
- **Existing document:** Locate the matching draft or topic before writing.
  Preserve established rules and source links; incorporate only supported
  additions and corrections within the requested scope.

Follow repository authority and explicit owner decisions for intended rules;
implementation is evidence of actual behavior. Report disagreements rather
than silently making either match the other. Keep intended rules, observed
behavior, and unresolved questions distinct. Never infer business rationale
from code or turn an agent suggestion into a confirmed decision.

Bind substantive rules and decisions to source IDs or links. For conversation
without stable links, capture the relevant speaker, request/correction, and
context in the document's source appendix or an existing task record. Mark this
as a conversation excerpt/summary, not an independently verified transcript.
If a source or rationale is absent, state the gap. Ask only about ambiguities
that materially affect the requested conclusion; continue supported sections.

## Write and retain

Read [templates/business.md](templates/business.md) when creating or
restructuring a document; routine updates need no template reload. Organize the
final understanding by business meaning. Retain only decision history that
explains a meaningful choice or change. Omit empty sections and technical-only
details. Keep confirmation, delivery, and verification separate; finishing a
task or generating a document proves none of them by itself.

Respect an explicit output target and repository conventions. Otherwise:

- With an existing task directory, write `docs/cases/<task-id>/business.md`.
- Without one, write `docs/business/archives/<YYYY-MM-DD>-<topic>.md`. Drafts
  may live there with explicit draft status; a directory name grants no status.
  Do not create task.md just to support business documentation. For distinct
  same-day topics/tasks with colliding names, add a meaningful suffix.
- If no writable workspace is available, or the user requests only text,
  return the document in the conversation and do not claim it was saved.

Update the same draft for the same work. Archive when the requested discussion
snapshot, task result, or historical investigation is complete; name that basis
and date. Archiving preserves a snapshot and does not imply all rules are
confirmed, delivered, verified, or authoritative. Retain unresolved items.
After archival, later business changes get a new document linked to the exact
rules they change; preserve the old body and add a follow-up link. Do not mark
an entire topic superseded when only one rule changed. Explicit corrections to
an archived document retain a dated correction note and source.
If the default destination already contains an archive, use the new task's
directory or a dated/specific filename beside it; never overwrite that snapshot
merely because the topic or task directory matches.

On saving, update the repository's existing business navigation, or create a
small `docs/business/README.md` when none exists. List topic, document, date,
draft/archive status, and specific relationships to earlier rules. Do not
duplicate the body or present the newest task as a complete current rulebook.
Update affected existing current business authority only within established
ownership and authorization. New authority or unresolved binding-rule changes
belong to setup-doc-governance if available, otherwise surface the decision;
ordinary drafting and archival do not depend on that Skill being installed.

## Finish

Check that the document is understandable without technical sources, material
rules and decisions are traceable, discarded requirements are not final rules,
time scopes are not mixed, conflicts and gaps are visible, and links resolve.
Report the saved location (or inline output), scope, and material uncertainties.
For workflow closure with no business change or clarification, report that fact
without creating an empty business document. An explicit historical summary
still warrants a document even when no business behavior changed.
