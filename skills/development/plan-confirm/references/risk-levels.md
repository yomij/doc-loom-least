# Risk Levels

| Level | Meaning | Anchoring |
|---|---|---|
| `low` | Small, reversible, local, well-verified change with no meaningful authority, public-contract, private-data, money, or destructive effect. | Update README wording. Fix a local copy error. Rename a local variable under tests. |
| `medium` | Reversible internal behavior, workflow/Skill text, internal API/data flow, or broader local change whose failure has a bounded recovery path. | Add an internal endpoint. Change an internal data structure. Refine an internal workflow rule without weakening authorization. |
| `high` | Security, money, private data, destructive/irreversible action, public compatibility, L1 critical fact, or any change that weakens authorization or enables such impact. | Change password hashing. Add billing behavior. Remove a public API. Drop a database column. Let plan approval authorize a new irreversible action. |

Classify from consequence, reversibility, exposure, authority impact, and
verification strength. Risk-sensitive topics still trigger `context-authority`
even when their final execution risk is medium.

Plan confirmation policy:

```yaml
plan_confirmation_policy:
  one_shot_low_risk_changes: no_case_no_plan_confirmation
  fast_path_low_risk_persistent_cases: approved_by_fast_path
  non_fast_path_low_risk_changes: require_confirmation
  medium_risk_changes: require_confirmation
  high_risk_changes: require_explicit_confirmation
```

Fast-path approval is allowed only when all conditions in
`shared-protocol.md` -> Fast-Path hold. `approved_by: fast-path` means the
agent verified those conditions under the current user request; it is not
permission for unrelated background execution.
