# pso uninstall

> Remove an installation using its inventory: standard keeps config/cache/logs, --complete removes everything PSO owns, --purge also deletes projects you name and confirm

Remove an installation using its inventory: standard keeps config/cache/logs, --complete removes everything PSO owns, --purge also deletes projects you name and confirm.

## Usage

```text
pso uninstall [--complete | --purge --project <dir> --confirm <same dir>] --root <dir> --yes [--trust <public.pem>] [--json]
```

## Flags

`--complete`, `--purge`, `--project`, `--confirm`, `--root`, `--yes`, `--trust`, `--json`.

## Examples

```bash
pso uninstall --yes --root C:Toolspso
pso uninstall --complete --yes --root C:Toolspso
pso uninstall --purge --project D:workdemo --confirm D:workdemo --yes --root C:Toolspso
```

## Failure cases

- Destructive: --yes is required. Only paths listed in the signed inventory are removed, links are never followed, and purge needs the project path twice plus a PSO marker file. Exit 1 lists residual items that could not be removed.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).
