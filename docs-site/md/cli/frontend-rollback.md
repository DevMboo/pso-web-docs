# pso frontend:rollback

> Return one MEI to its previous (or a chosen) published release, after the same compatibility checks; the other microfrontends and the FEI are not touched

Return one MEI to its previous (or a chosen) published release, after the same compatibility checks; the other microfrontends and the FEI are not touched.

## Usage

```text
pso frontend:rollback <mei> --site <dir> | --env <name> [--to 1.1.0] [--json]
```

## Flags

`--site`, `--env`, `--to`, `--json`.

## Examples

```bash
pso frontend:rollback customers --site ./site
pso frontend:rollback customers --site ./site --to 0.1.0
```

## What it changes

- Points one MEI back to its previous (or a chosen) published version after the same compatibility and signature checks. The shell and the other MEIs are not touched.

## Failure cases

- `has no earlier release`, `is not published on this site`, `is already active`.
- A version that is no longer compatible with the active shell is refused.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [frontend/releases](frontend/releases.md)
