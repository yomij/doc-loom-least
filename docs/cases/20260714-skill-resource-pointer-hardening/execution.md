---
case_id: 20260714-skill-resource-pointer-hardening
status: ready_to_close
updated_at: 2026-07-14T20:40:29+0800
---

# Execution Report

## Human Summary

- Outcome so far: implementation, review fix, exact-format verification, and
  independent Engineering/Spec re-review all pass. The case is ready to close.
- What changed: seven resource-consuming canonical Skills now expose 18 exact
  Markdown links while preserving their existing read triggers. One portable
  authoring convention was added to `skills/README.md`; resource files and
  symlinks are unchanged.
- User action needed: none during execution.
- Local Git effect: branch `codex/skill-resource-pointer-hardening`; approved
  plan commits `0d0b9c31b0a4f33f1eebc4508587524db6b01702` and
  `7667aebfde747735466a5d532d8dc4429f95202c`, plus reference-format amendment
  `67d4b68`, exact-format amendment `e8d0bf5`, and review-strategy amendment
  `95f9d05`. Green task commit:
  `27d76331c9c9aacf71ded9238cdeb99633143e8f`. No push or installed-cache update
  is authorized.

## Plan Reference

- plan_version: 5
- base_commit: `b0505d15c0cae72740f2a3e54a0c48002db13b36`
- adaptive_execution: disabled; material deviations return to planning.

## Plan Followed

- Full-flow branch mode is active on the declared branch.
- The approved TDD exception is valid: this is a Skill/Markdown behavior fix
  without an executable workflow interpreter.
- Plan amendment v2 removed a repeated per-Skill resource-base sentence after
  the user identified it as unnecessary. The direct `./` links and single
  maintainer convention remain the complete approved solution.
- Plan amendment v3 standardizes reference pointers as grammatical
  `read the [descriptive label](./references/file.md)` wording while leaving
  reference bodies content-driven.
- Plan amendment v4 supersedes that prose form with the exact
  `setup-doc-governance` resource-condition list selected by the user and
  removes all `Read the` pointer text.
- Plan amendment v5 adds an explicitly requested independent review subagent;
  the main agent retains final review ownership.
- No unapproved deviation is recorded.

## Characterization

- Current baseline Skills contained 18 bare code-span resource pointers across
  17 lines and seven resource-consuming Skills.
- The reproduced wrong-base read omitted the owning Skill directory and tried
  `skills/development/references/shared-protocol.md`; the correct consumer path
  is `skills/development/docloom-workflow/references/shared-protocol.md`.
- The shared-protocol path resolves successfully through each existing local
  symlink. `find -L skills -type l -print` returned no broken symlink.
- The current resource tree contains eight valid local symlinks and twelve real
  canonical reference/template files. This task changes neither count nor
  target.
- Plain `rg --files` can omit symlink entries; pointer verification therefore
  uses the exact Markdown target joined to the owning `SKILL.md` directory,
  followed by `test -e` and canonical resolution.

## Changes Made

- Converted all 18 canonical local resource pointers to direct Markdown links
  beginning with `./references/` or `./templates/`.
- Normalized every resource consumer to `Read when trigger condition is met:`
  plus `[Resource](./references/or/templates/file.md): trigger` bullets, using
  `setup-doc-governance` as the format baseline.
- Preserved every mandatory/conditional read boundary, including discovery,
  TDD-exception, complexity-only, governance, handoff, execution, and closure
  branches.
- Added one maintainer rule to `skills/README.md`; no per-Skill explanatory
  sentence remains after approved plan amendment v2.
- Changed no resource file, symlink, shared protocol, authority document,
  workflow stage, authorization rule, dependency, script, or CI surface.

## TDD Applicability

- TDD Required: No.
- Exception category: Skill/docs behavior change.
- Alternative verification: characterize all current pointer forms and the
  reproduced failure; apply the minimal pointer-only change; verify explicit
  links, exact target resolution, symlink/copy readability, semantic anchors,
  diff scope, and Post-execution Engineering/Spec review.

## Acceptance Criteria Status

| Criterion | Status | Evidence |
|---|---|---|
| Direct relative Markdown resource links | met | Seven consumers have one condition label each; 18 links are 18 valid `[Resource](target): trigger` bullets; zero bare paths or `Read the` pointer prose. |
| Every target resolves from owning Skill | met | All 18 extracted targets pass `test -e` and canonical resolution. |
| Shared single-source and symlink integrity | met | Eight unchanged symlinks; no broken link. |
| Copy installation readability | met | `cp -RL` simulation contains no symlink and resolves all 18 links. |
| Workflow semantics unchanged | met | F1 restored the exact plan-template draft-writing condition; re-review confirms all 18 triggers preserve baseline semantics. |
| Portable maintainer guidance | met | One convention in `skills/README.md`; no absolute or direct `_shared` consumer path. |
| Post-execution review | met | Independent subagent and main-agent re-review: Engineering pass, Spec pass, aggregate pass, no remaining finding or material gap. |

## Commands Run

| Command | Result | Notes |
|---|---|---|
| `git switch -c codex/skill-resource-pointer-hardening` | passed | Created declared branch with draft case files preserved. |
| Plan stage/diff/check/commit sequence | passed | Commit `0d0b9c3`; title and Doc Loom trailers verified by commit output. |
| Bare pointer inventory with `rg` | characterized | 18 pointers across 17 lines and seven Skills. |
| `find skills -type l` plus `readlink` | passed | Eight existing symlinks recorded. |
| `find -L skills -type l -print` | passed | No broken symlink. |
| Resource file inventory | passed | Existing shared/private resources recorded; no topology change planned. |
| Initial link-resolution harness | failed, corrected | macOS lacked `realpath`; zsh special variable `path` also overwrote command lookup. No Skill result was involved. |
| Second link-resolution harness | failed, corrected | zsh command substitution did not split multiple links by newline. Replaced with line-oriented reads. |
| Final bare-pointer and direct-link checks | passed | Zero bare pointers; all 18 Markdown links enumerated and resolved. |
| `find -L skills -type l -print` and symlink count | passed | No broken link; eight unchanged symlinks. |
| Temporary `cp -RL` simulation | passed | No copied symlink; all 18 dereferenced resource links exist. |
| Eight `quick_validate.py` runs via `uv --with pyyaml` | passed | Every canonical Skill is structurally valid. |
| Semantic-anchor and forbidden-path checks | passed | Required workflow anchors remain; no absolute path or direct `_shared` consumer instruction. |
| `git diff --check` | passed | No whitespace error. |
| Exact `setup-doc-governance` format check | passed | Seven condition labels, 18 resource bullets, 18 resolved targets, zero rejected pointer forms. |
| Task stage/diff/check/commit sequence | passed | Green task commit `27d76331c9c9aacf71ded9238cdeb99633143e8f`; explicit paths only. |
| `skillshare audit ./skills --format json --yes` | passed | Clean; risk score 0 and no finding. |

## Review Risk

`low`: initial Important F1 and three Minor issues/evidence gaps were corrected
in `7c984b4`; independent and main re-review pass with no remaining finding or
material gap.

## Post-Execution Review Strategy

- Invoke the installed `review` Skill in `Post-execution` mode after the green
  task commit.
- Spawn one independent review subagent with the approved plan, authority,
  committed delta, checks, and case evidence; do not provide the intended
  verdict.
- The main agent verifies the complete target, owns Engineering/Spec severity
  and deduplication, persists review evidence, and runs any fix/re-review loop.

## Commits

| Step | Commit |
|---|---|
| plan | `0d0b9c31b0a4f33f1eebc4508587524db6b01702` |
| plan-amendment v2 | `7667aebfde747735466a5d532d8dc4429f95202c` |
| plan-amendment v3 | `67d4b68` |
| plan-amendment v4 | `e8d0bf5` |
| plan-amendment v5 | `95f9d05` |
| task:pointer-hardening | `27d76331c9c9aacf71ded9238cdeb99633143e8f` |
| review-fix:F1-F3 | `7c984b487fc876516ae86697295e99abdcc7abc5` |

## Reproducible Verification

Pointer shape and target resolution:

```zsh
test "$(rg -l '\]\(\./(?:references|templates)/' skills/**/SKILL.md | wc -l | tr -d ' ')" -eq 7
test "$(rg -l '^Read when trigger condition is met:$' skills/**/SKILL.md | wc -l | tr -d ' ')" -eq 7
test "$(rg -n '^- \[[^]]+\]\(\./(?:references|templates)/[^)]+\):' skills/**/SKILL.md | wc -l | tr -d ' ')" -eq 18
! rg -n '`(?:references|templates)/[^`]+`|Read the \[[^]]+\]\(\./(?:references|templates)/' skills/**/SKILL.md

for skill in $(find skills -name SKILL.md -type f | sort); do
  dir=${skill%/*}
  while IFS= read -r target; do
    test -n "$target" || continue
    test -e "$dir/${target#./}"
  done < <(rg -o '\]\(\./(?:references|templates)/[^)]+\)' "$skill" | sed -E 's/^\]\((.*)\)$/\1/' || true)
done
```

Distribution and validation:

```zsh
test -z "$(find -L skills -type l -print)"
test "$(find skills -type l -print | wc -l | tr -d ' ')" -eq 8

tmp=$(mktemp -d)
cp -RL skills "$tmp/skills"
test -z "$(find "$tmp/skills" -type l -print)"
for skill in $(find "$tmp/skills" -name SKILL.md -type f | sort); do
  dir=${skill%/*}
  while IFS= read -r target; do
    test -n "$target" || continue
    test -e "$dir/${target#./}"
  done < <(rg -o '\]\(\./(?:references|templates)/[^)]+\)' "$skill" | sed -E 's/^\]\((.*)\)$/\1/' || true)
done
rm -rf "$tmp"

for skill_dir in $(find skills -name SKILL.md -type f -exec dirname {} \; | sort); do
  uv run --quiet --with pyyaml python /Users/yomi/.codex/skills/.system/skill-creator/scripts/quick_validate.py "$skill_dir"
done

skillshare audit ./skills --format json --yes
git diff --check
```

## Post-Execution Review

Initial independent subagent review against exact baseline
`95f9d0560cbc22b1e939a34c3eba64ec2890dbac`:

| Axis | Verdict | Findings / gaps |
|---|---|---|
| Engineering | pass | No Critical/Important. Minor: execution evidence named harnesses without exact reproducible commands. |
| Spec | changes_required | Important F1: `plan-confirm` expanded the plan-template condition to validation, violating the unchanged conditional-loading boundary. |
| Aggregate | changes_required | F1 blocks readiness; axes do not compensate. |

Main-agent additions:

- Minor F2: `skills/README.md` used the invalid illustrative path
  `./references/or/templates/file.md`.
- Minor F3: `tdd-execute` repeated the execution-template condition after the
  new resource list.

Review-fix disposition:

- F1: restored `Plan template` to writing `plan.md` as `status: draft`.
- F2: replaced the pseudo-path with valid reference and template examples.
- F3: removed the duplicate execution-template sentence.
- Engineering evidence gap: added the reproducible commands above.
- Review-fix commit: `7c984b487fc876516ae86697295e99abdcc7abc5`.
- Re-review: Engineering `pass`, Spec `pass`, aggregate `pass` against exact
  baseline `95f9d0560cbc22b1e939a34c3eba64ec2890dbac` through task
  `27d76331c9c9aacf71ded9238cdeb99633143e8f` and review fix
  `7c984b487fc876516ae86697295e99abdcc7abc5`.
- Final findings: none within reviewed scope.
- Final evidence gaps: none material.
