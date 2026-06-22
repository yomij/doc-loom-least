# Risk Levels

| Level | Meaning | Anchoring |
|---|---|---|
| `low` | Documentation, local copy, non-critical tests, or behavior-preserving refactor. | Update a README section. Fix a typo in a config comment. Rename a local variable. |
| `medium` | Normal feature work, local behavior change, internal API, or local data flow change. | Add a new CLI flag. Change an internal data structure. Add a new internal endpoint. |
| `high` | Security, auth, permission, billing, privacy, data deletion, public API, schema migration, destructive architecture boundary, or L1 critical fact. | Change password hashing. Add a billing tier. Remove a public API endpoint. Drop a database column. |

Plan confirmation policy:

```yaml
plan_confirmation_policy:
  low_risk_changes: require_confirmation
  medium_risk_changes: require_confirmation
  high_risk_changes: require_explicit_confirmation
```

Distributed Doc Loom skills do not support automatic execution for low-risk
changes.
