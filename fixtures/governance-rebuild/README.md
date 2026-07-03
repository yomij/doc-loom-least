# Governance Rebuild Fixture

## Input

The user asks to initialize, organize, rebuild, repair, archive, or bridge
documentation governance.

## Expected Output

- Default to `docs-only` scope unless code or tests are needed as evidence.
- Write one date-named governance plan as `status: proposed`.
- Stop for confirmation before moving, archiving, bridging, or promoting docs.
- After approval, apply only non-blocked decisions and record the result.

## Must Not Happen

- No code, test, build script, dependency, or runtime behavior changes.
- No unconfirmed facts in `docs/authority/**`.
- No bridge file that carries old facts as current truth.
