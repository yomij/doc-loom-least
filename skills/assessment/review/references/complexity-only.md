# Complexity-only Review

Use this reference only when `review` selects `Complexity-only` or the
complexity section of `Dual-pass`.

## Scope

Find unnecessary complexity only. Do not report correctness, security,
performance, or coverage findings here. If those concerns matter, say they need
Standard review.

The best outcome is a shorter diff, fewer docs, fewer branches, fewer concepts,
or fewer workflow moving parts.

## Tags

- `delete:` dead code, unused flexibility, speculative feature, duplicate docs,
  duplicate workflow, or unused compatibility path. Replacement: nothing.
- `stdlib:` hand-rolled behavior the language standard library already
  provides. Name the standard feature.
- `native:` dependency or local mechanism replacing platform-native behavior.
  Name the native feature.
- `yagni:` abstraction with one implementation, config nobody sets, layer with
  one caller, or future extension without current requirement.
- `shrink:` same behavior or claim can be expressed with fewer lines, branches,
  files, or sections.
- `governance-bloat:` Doc Loom-specific process, artifact, status field,
  authority layer, or workflow stage that does not solve a real governance
  failure.

## Do Not Flag

- A single smoke test or assert-based self-check.
- Tests that prevent realistic regression.
- Compatibility required by an active public contract.
- Error handling for real external input.
- Current authority or ADR source-of-truth text.
- Duplication required by distinct ownership boundaries.

## Output

Use this structure:

```md
## Scope

## Evidence Reviewed
| Source | Why Included |
|---|---|

## Complexity Findings

`file:Lx-Ly: <tag> <what to cut>. <replacement>.`

## Net

net: -<N> lines possible.
```

Each finding must be one line, locatable, and replacement-oriented.

Examples:

```text
repo.py:L88: yagni: AbstractRepository with one implementation. Inline it until a second implementation exists.
date.ts:L4: native: dayjs imported for one format call. Intl.DateTimeFormat, 0 deps.
docs/design.md:L52-L71: governance-bloat: new review stage only makes reasoning feel safer. Keep review as conversation-only mode.
```

If there is nothing to cut, say `Lean already. Ship.` and stop.

Estimate `net` from removable lines when practical. For documentation, count
lines or sections. If line count is not reasonably knowable, write
`net: reduction possible, line count not estimated.`
