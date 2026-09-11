# Governance Placement and Migration

## Placement

| Content | Default target |
|---|---|
| Confirmed reusable rules | `docs/authority/` |
| Task decisions and evidence | `docs/cases/<case-id>/` |
| Business snapshots under the business-docs contract | Existing task's `business.md` or `docs/business/archives/`; explicit draft/archive state, no automatic authority |
| Business navigation | Existing business index or `docs/business/README.md`; derived |
| Navigation and derived explanations | Existing documentation index or guides |
| Superseded material | `docs/archive/` |
| Unverified drafts | `docs/scratch/` or case notes |

Within authority, use `constitution.md` for foundational principles and
`product/`, `domain/`, `architecture/`, `contracts/`, `workflow/`, `agent/`, or
`operations/` for the corresponding confirmed facts. Preserve an existing ADR
location for decisions. Create only locations needed by actual content.

## Entries and Migrations

Update the existing documentation index. Link affected authority, evidence,
governance records, archives, and derived views; label their roles.

Use a bridge only when moving an established entry would otherwise misdirect
readers. Keep just its superseded status, current target, and historical source;
do not copy old facts into the bridge.

Preserve historical content when archiving. Mark it archived or superseded and
provide a replacement link through `superseded_by` or an archive index when a
replacement exists. Check incoming links after moves. A directory name alone
neither grants authority nor settles a conflict.
