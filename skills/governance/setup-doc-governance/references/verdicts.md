# Governance Verdicts

Use the same verdict set for files and facts:

```text
promote
merge
bridge
archive
block
```

## File-Level Verdicts

| Verdict | Meaning |
|---|---|
| `promote` | Current facts in the source will be extracted into a new authority doc. |
| `merge` | Current facts in the source will be merged into an existing authority doc. |
| `bridge` | Keep a thin entry pointing to current authority or archive to prevent future misread. |
| `archive` | Move to archive; no longer current fact. |
| `block` | Conflict, high risk, or insufficient evidence requires user or owner decision. |

## Fact-Level Verdicts

| Verdict | Meaning |
|---|---|
| `promote` | Write the fact into a new authority doc. |
| `merge` | Merge the fact into an existing authority doc. |
| `bridge` | The fact itself is not promoted, but the source file needs a bridge. |
| `archive` | The fact has historical value only. |
| `block` | Fact is conflicting, under-evidenced, or high risk. |

Do not introduce `candidate`, `accepted`, `canonical`, or `deprecated` as a
default state matrix.

## Code And Test Evidence

Code and tests may be read as evidence in `full-repo` scope. They are never
modified by this skill.

Code plus tests can override historical docs for low-risk internal
implementation details when:

- The topic is not a High-Risk Topic.
- The historical doc is not active authority.
- Code and tests agree.

Use `block` when:

- A High-Risk Topic is involved.
- An ADR defines the boundary.
- A user or owner recently confirmed the fact.
- Code and tests disagree.
- Test coverage is missing and impact is high.

## Plan Tables

Use the table structures in `../templates/GOVERNANCE_PLAN.md`.
