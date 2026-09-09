---
status: active
authority: true
layer: authority
type: operations
source_of_truth: code
supersedes: []
superseded_by: []
owner: user
last_verified: 2026-09-09
---

# Distribution

Doc Loom Least is distributed as four Agent Skills through skillshare:

- docloom-workflow
- review
- grill
- setup-doc-governance

The former context-authority, plan-confirm, tdd-execute, and doc-sync-close
entry points are retired by ADR-0004. Existing installations are not changed by
a repository update alone; users should sync the new source and remove stale
copies according to their skillshare setup.

Canonical discovery is recursive under skills/. Archived docs are not canonical.
No shared protocol, handoff template, or cross-skill symlink is required by the
current implementation. Review and grill are independent manual helpers;
docloom-workflow is the normal entry.
