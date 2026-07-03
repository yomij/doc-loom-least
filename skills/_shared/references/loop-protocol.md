# Loop Protocol

Use this reference when `docloom-workflow` handles loop-style discovery,
status, continuation, or next-slice requests.

This protocol is implementation guidance below the constitution, authority
docs, and shared protocol. It does not create a new workflow stage.

## Candidate Meaning

A candidate is a proposed next work object for human selection, not a case,
plan, approval, or execution queue item. Case candidates choose existing
workflow continuation; next-slice candidates choose the next small product or
development slice.

## Case Candidates

Use status-only routing, shared case identity, `case_state.yaml`, and case
artifacts to find case candidates. `docs/cases/README.md` may narrow which
artifacts to read, but routing truth remains `case_state.yaml` and the
artifact-derived phase rules in shared protocol.

Use this output shape when multiple case candidates or follow-ups exist:

```md
## Case Candidates

| ID | Case | Phase | Evidence | Next Skill | Required Input | Why Next |
|---|---|---|---|---|---|---|
```

## Next-Slice Discovery

Use next-slice discovery when the user asks what to build next, what feature
slice should follow, or asks the agent to find the next iteration target.

Required input is `docs/product/current-state.md` or equivalent current-turn
facts. Required fields: Product Goal, Target User, Current Demo State,
Constraints, Do Not Build Yet. Optional ranking signals: Current Bottleneck,
Feedback / Signals.

If a required field is missing, do not invent a product direction. Return:

```text
Blocking issue: missing product current state.
Required input: Product Goal, Target User, Current Demo State, Constraints, and Do Not Build Yet.
```

Discover candidates from Product Goal minus Current Demo State, Current
Bottleneck, Feedback / Signals, closure follow-ups, TODO/FIXME, failing checks,
incomplete adjacent implementation, and Constraints / Do Not Build Yet
exclusions.

Score candidates with:

```text
Score = Impact + Confidence + Unlock - Effort - Risk
```

Use integer scores from 1 to 5. `Unlock` measures downstream work enabled;
`Effort` and `Risk` are penalties. Sort by Score descending; break ties with
higher Confidence, then lower Risk, then lower Effort. Explain the
recommendation in one sentence. Keep factor scores out of the default output;
expand them only when the user asks why, when the score is challenged, or when
recording case evidence.

Output:

```md
## Next Slice Candidates

| ID | Slice | Why | Score | Route |
|---|---|---|---:|---|
```

Then state:

```text
Recommended: N1: <slice>
Reason: <one sentence>
Decision: reply with a candidate ID, "more", or updated product-state facts.
```

## Safety

- A recommendation is not execution authorization and does not approve a plan.
  After selection, plan through case identity and `plan-confirm`; execute only
  through `tdd-execute` after the plan is approved.
- Shared-protocol routing and confirmation rules still apply, including no
  auto-trigger of `review` or `grill`, no silent authority promotion, and
  `context-authority` for high-risk, authority-touching, or conflict candidates.
