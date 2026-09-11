---
status: active
authority: true
layer: authority
type: workflow
source_of_truth: user_confirmed
supersedes: []
superseded_by: []
owner: user
last_verified: 2026-09-11
---

# Business Documentation

business-docs independently organizes business rules and decisions from
conversations, completed tasks, and historical evidence for general business
readers, including people without a technical background. It requires neither
docloom-workflow nor task.md.
It also updates existing business documents within their established ownership.
docloom-workflow calls it when business conclusions arise or change and
finalizes the document at closure. Technical-only closure needs no empty archive.

Business prose describes actors, conditions, outcomes, exceptions, scope, and
sourced decisions. Technical evidence belongs in the source appendix. Distinguish
intended rules from observed implementation, current from historical behavior,
and confirmation from delivery and verification. Missing rationale remains
unknown; conflicts and missing evidence stay visible. Conversation summaries
without stable links preserve the relevant speaker and context as excerpts or
summaries, not independently verified transcripts.

Adapt to the requested audience and purpose. The body must stand alone without
project or technical knowledge: explain necessary terms and business activities,
preserving exact limits and exceptions. Describe technical constraints by their
business effects; keep technical identifiers in the source appendix. Label
illustrative examples without adding rules. Delivery tracking is conditional.

Respect explicit targets and existing repository conventions. Otherwise use an
existing task's business.md, or docs/business/archives/YYYY-MM-DD-topic.md without
creating a task record. Draft/archive state is explicit and independent of
directory name, rule confirmation, delivery, verification, and authority.
Archival preserves a completed discussion snapshot, task result, or investigation.
Reuse matching drafts; preserve archived bodies and link later rule changes.
Use the existing business index, otherwise docs/business/README.md, as derived
navigation. It is not a complete current rulebook. Existing current business
authority is updated only within its established ownership and authorization.
Creating or promoting new authority remains governance work.

## Sources

- [ADR-0005](../../adr/ADR-0005-independent-business-docs.md)
- [2026-09-11 audience clarification](../../business/archives/2026-09-11-business-docs-general-audience.md#来源与查证说明)
- [Skill implementation](../../../skills/product/business-docs/SKILL.md)
