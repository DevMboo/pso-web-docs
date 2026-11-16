# pso dispatch:remove

> Remove the installed dispatch provider from a service and disable dispatch (event declarations and handlers are never deleted)

Remove the installed dispatch provider from a service and disable dispatch (event declarations and handlers are never deleted).

## Usage

```text
pso dispatch:remove [--service <name>] [--dry-run] [--json]
```

## Flags

`--service`, `--dry-run`, `--json`.

## Examples

```bash
pso dispatch:remove --service orders --dry-run
pso dispatch:remove --service orders
```

## Failure cases

- Declarations and handlers stay on disk; removal changes the adapter and enabled state, not the event contract.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [dispatch/installation](dispatch/installation.md)
