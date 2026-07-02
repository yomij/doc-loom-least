# Risk Levels

| Level | Meaning | Anchoring |
|---|---|---|
| `low` | Documentation, local copy, non-critical tests, or behavior-preserving refactor. | Update a README section. Fix a typo in a config comment. Rename a local variable. |
| `medium` | Normal feature work, local behavior change, internal API, or local data flow change. | Add a new CLI flag. Change an internal data structure. Add a new internal endpoint. |
| `high` | Any High-Risk Topic from `shared-protocol.md`, destructive architecture boundary, or L1 critical fact. | Change password hashing. Add a billing tier. Remove a public API endpoint. Drop a database column. |

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
