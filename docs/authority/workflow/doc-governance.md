---
status: active
authority: true
layer: authority
type: workflow
source_of_truth: code
supersedes: []
superseded_by: []
owner: user
last_verified: 2026-09-10
---

# Documentation Governance

setup-doc-governance owns documentation authority, hierarchy, knowledge
promotion, and current/historical classification. Ordinary document edits stay
with docloom-workflow; business writing and archival use business-docs. The
governance Skill contains its core rules; placement and migration
references and the detailed plan template are conditional resources.

Choose target_scope independently from evidence_scope. Target is current-case
or repository and bounds documentation changes. Evidence defaults to docs-only;
repo-readonly additionally permits code, tests, and configuration as evidence
when needed and allowed. Reading repository evidence never grants write scope.

Follow declared authority precedence and source ownership. Confirmed current
rules constrain later work; implementation describes actual behavior. An active
marker alone is not authority, and missing metadata alone does not invalidate an
owner-designated rule. Cases provide task evidence, derived documents route or
summarize, archives preserve history, and scratch is unverified. New authority
must identify its source, status, ownership, and verification date.

Business documents follow the [business documentation contract](business-docs.md).
Draft/archive labels describe document maturity, not confirmation, delivery,
verification, or authority. A business archive preserves a sourced snapshot;
business navigation is derived. Neither automatically becomes current authority.
Routine drafting, archival, and follow-up links under this approved contract
need no fresh governance decision. New authority promotion still follows the
rules below.

Governance verdicts are promote, merge, bridge, archive, and block. Resolve
conflicts through declared precedence or existing owner decisions. Block only
decisions that lack evidence, ownership, or authorization; do not silently
promote inference or rewrite current rules to match implementation drift.

Concrete structural, authority, archive, and lifecycle decisions require owner
authorization. Reuse existing approval that covers them; a general setup request
allows investigation and preparation but does not approve unspecified binding
rules. New approval is needed only for changes beyond prior scope or effects.
Routine derived updates inside authorized work need no additional approval.

Record applied governance in the current task record or a standalone governance
batch; use the plan template only for multiple decisions needing tabular detail.
Preserve completed batches. Refresh affected indices and verify source
traceability, metadata, links, and entry priority. Report applied and blocked
decisions; do not claim setup complete with requested changes still blocked.

Preserve the constitution. Creating, amending, or moving it needs an explicit
owner decision and coordinated index, SSOT map, and decision-log updates. Do not
create empty authority sections or new lifecycle areas for structural symmetry.
