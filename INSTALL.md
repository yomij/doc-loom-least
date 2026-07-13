# Install Doc Loom Least Skills

Doc Loom Least is distributed as Agent Skills. The fastest supported path is
`skillshare`, which can install, sync, check, and update the skills across AI
CLI targets.

## Prerequisites

Install and initialize `skillshare` once on the machine:

```bash
skillshare init --no-copy --targets codex --git --skill
```

If `skillshare` is already initialized, check the current target setup:

```bash
skillshare status
```

## Install

For a public GitHub repository:

```bash
skillshare install github.com/yomij/doc-loom-least --track --json
skillshare sync
```

For a private repository using SSH:

```bash
skillshare install git@github.com:yomij/doc-loom-least.git --track --json
skillshare sync
```

`--json` makes the install non-interactive and installs all discoverable skills.
This repository publishes canonical skills only from the grouped `skills/`
tree. `.skillignore` excludes `docs/archive/**`, and archived reference skills
are no longer kept under `docs/archive/raw/reference/`.

## Install Into A Project

To vendor the skills into a specific repository instead of the global
skillshare source:

```bash
skillshare init -p --targets codex
skillshare install git@github.com:yomij/doc-loom-least.git --track --json -p
skillshare sync -p
```

Use the HTTPS `github.com/yomij/doc-loom-least` source instead of SSH when the
repository is public or the local git credential helper is already configured.

## Update

Check for updates:

```bash
skillshare check
```

Update all tracked skills and sync them to configured targets:

```bash
skillshare update --all --diff
skillshare sync
```

For project mode:

```bash
skillshare check -p
skillshare update --all --diff -p
skillshare sync -p
```

## Verify

List installed skills:

```bash
skillshare list --json
```

Run the security audit on this repository's skill source:

```bash
skillshare audit ./skills --format json
```

Expected canonical skill frontmatter names:

- `docloom-workflow`
- `setup-doc-governance`
- `context-authority`
- `plan-confirm`
- `tdd-execute`
- `doc-sync-close`
- `review`
- `grill`

Current source paths:

| Group | Skills |
|---|---|
| `skills/development/` | `docloom-workflow`, `context-authority`, `plan-confirm`, `tdd-execute`, `doc-sync-close` |
| `skills/governance/` | `setup-doc-governance` |
| `skills/assessment/` | `review`, `grill` |

## Sync Mode

The skills support both `merge` and `copy` target sync modes.

`merge` is the default:

```bash
skillshare target codex --mode merge
skillshare sync --force
```

For targets that cannot follow symlinks, use `copy`:

```bash
skillshare target codex --mode copy
skillshare sync --force
```

Doc Loom Least keeps only multi-owner file contracts under `skills/_shared/`.
The cross-Skill shared protocol is exposed to its consumers through local
relative symlinks; the shared handoff template is exposed only to its direct
writers. Single-owner references and templates are real private files inside
their owning Skill. In `merge` mode, shared symlinks remain connected to the
tracked repository. In `copy` mode, `skillshare` copies each symlink target as a
real file into the copied Skill, so all required resources remain readable when
the target does not support symlinks.

Runtime companion Skills are invoked by canonical frontmatter name, not by
filesystem path. The supported repository installation discovers all canonical
Skills together, including the `tdd-execute` -> `review` Post-execution
relationship.

## Notes

- Always run `skillshare sync` after install or update.
- Use `GITHUB_TOKEN`, `SKILLSHARE_GIT_TOKEN`, SSH, or a git credential helper
  for private repositories.
- Do not install from a local repository path when you need multi-skill
  discovery. `skillshare install <local-path>` treats that directory as one
  skill. Use a git URL, SSH URL, or `file://` git URL for repository discovery.
