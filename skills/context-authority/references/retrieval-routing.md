# Retrieval Routing

Read by relevance. Do not build a full context pack.

## Governance Docs

Read `docs/README.md` when a docs entry or authority
route is needed.

Read governance plan files when the task involves:

- Documentation governance.
- Authority update.
- Workflow or agent policy.
- Public contract.
- High risk.
- User-provided new facts that may touch blocked decisions.
- Missing authority or conflict that requires blocked-decision context.

If a case is active, read `docs/cases/<case-id>/governance-plan.md`.
Otherwise read the most recent `docs/governance/*.md` with
`status: approved` or `status: applied_with_blocks`.
If blocked-decision context is needed, also read any
`applied_with_blocks` plan.
If `docs/governance/GOVERNANCE_PLAN.md` exists from an older design, read it
as legacy context only.

If governance or authority is missing:

- Low-risk local work may continue with `proceed_to_plan_with_risk`.
- High-risk, public contract, workflow, agent policy, architecture boundary,
  security, privacy, billing, or data deletion work should route to
  `run_setup_doc_governance` or `needs_user_decision`.

## Authority Filtering

Include active authority docs only by default.

Exclude:

- `status: archived`
- `status: superseded`
- `status: needs_refresh`
- `source_of_truth: generated` as a fact source

Generated docs can be consumed as views only.

## Case Docs

Read existing case docs only when resuming or when the case is explicit:

```text
case_state.yaml
plan.md
handoff.md
execution.md
closure.md
```

`case_state.yaml` is a cache. If it conflicts with Markdown, report the
conflict and derive phase from Markdown plus git state.

## Code And Tests

Read code/tests when the intent requires them. Use:

- User keywords.
- Authority frontmatter `related_code` and `related_tests`.
- `rg` for symbols, file names, errors, config keys.
- Test naming conventions and nearby tests.

Prefer exact identifiers and direct call/test paths before semantic browsing.
Every included source must have `why_included`.

## Context Source Notes

Use this shape inline or in the brief:

```yaml
context_sources:
  case:
    existing_id:
    proposed_slug:
    intent:
    risk:
  included:
    - path:
      layer:
      why_included:
      trust:
  excluded:
    - path:
      reason:
```

Runtime evidence, logs, CI results, or user-provided outputs need a time window
and trust note.
