# pso frontend:releases

> Show the published versions of each MEI and of the FEI on a site root: which is active and the activation history

Show the published versions of each MEI and of the FEI on a site root: which is active and the activation history.

## Usage

```text
pso frontend:releases [<mei>] --site <dir> [--json]
```

## Flags

`--site`, `--json`.

## Examples

```bash
pso frontend:releases --site ./site
pso frontend:releases customers --site ./site --json
```

## What it changes

- Shows the active shell and, for each MEI, the active version, the published versions and the activation history.

## Failure cases

- `--site <directory>` is required.
- A site with nothing published shows no active FEI and no versions; it is not an error.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [frontend/releases](frontend/releases.md)
