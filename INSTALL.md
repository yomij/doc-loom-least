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
This repository uses `.skillignore` so only the canonical skills under the
grouped `skills/` tree are published; reference materials under
`docs/reference/skills/` are not published or synced to targets.

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

Doc Loom Least keeps the source of truth for shared workflow rules at
`skills/_shared/references/shared-protocol.md`. Each grouped skill that needs
those rules exposes `references/shared-protocol.md` as a relative symlink to
that source. In `merge` mode, the symlink remains connected to the tracked
repository. In `copy` mode, `skillshare` copies the symlink target as a real
file into each copied skill, so the reference remains readable even when the
target does not support symlinks.

## Notes

- Always run `skillshare sync` after install or update.
- Use `GITHUB_TOKEN`, `SKILLSHARE_GIT_TOKEN`, SSH, or a git credential helper
  for private repositories.
- Do not install from a local repository path when you need multi-skill
  discovery. `skillshare install <local-path>` treats that directory as one
  skill. Use a git URL, SSH URL, or `file://` git URL for repository discovery.
