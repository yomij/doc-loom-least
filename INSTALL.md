# Install Doc Loom Least Skills

Doc Loom Least is distributed as four Agent Skills:
docloom-workflow, review, grill, and setup-doc-governance.

## Install

For a public repository:

    skillshare install github.com/yomij/doc-loom-least --track --json
    skillshare sync

For a private repository:

    skillshare install git@github.com:yomij/doc-loom-least.git --track --json
    skillshare sync

The repository source is the canonical set. Existing installations are not
automatically cleaned up; after syncing, remove stale copies of the retired
context-authority, plan-confirm, tdd-execute, and doc-sync-close Skills through
your normal skillshare configuration.

## Update and verify

    skillshare check
    skillshare update --all --diff
    skillshare sync
    skillshare list --json
    skillshare audit ./skills --format json

For project-scoped installation, add -p to the install and sync commands.
Use skillshare's normal target mode for copy or symlink targets. Do not modify a
global installation as part of repository-only changes.

## Manual fallback

On a system without skillshare, copy or symlink the four directories containing
SKILL.md under skills/ into the target Skills directory. Keep each directory's
supporting references beside its owner. The normal entry is docloom-workflow;
call review or grill explicitly when needed.
