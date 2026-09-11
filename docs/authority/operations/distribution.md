---
status: active
authority: true
layer: authority
type: operations
source_of_truth: code
supersedes: []
superseded_by: []
owner: user
last_verified: 2026-09-10
---

# Distribution

Doc Loom Least is distributed as five Agent Skills through skillshare:

- docloom-workflow
- business-docs
- review
- grill
- setup-doc-governance

The former context-authority, plan-confirm, tdd-execute, and doc-sync-close
entry points are retired by ADR-0004. Existing installations are not changed by
a repository update alone; users should sync the new source and remove stale
copies according to their skillshare setup.

Canonical discovery is recursive under skills/. Archived docs are not canonical.
No shared protocol, handoff template, or cross-skill symlink is required by the
current implementation. Review is a separate read-only capability; grill requires
explicit challenge intent. docloom-workflow is the normal entry. Each installed
Skill carries its own core rules; README files are not runtime dependencies.

ADR-0005 adds business-docs as an independent entry. Install it alongside
docloom-workflow for business archival during development closure. It can also
be used alone with its local template; it needs no task record or sibling path.
