# pso dispatch:lock

> Record the current event contracts in pso.dispatch.lock.json so later schema changes are checked for compatibility

Record the current event contracts in pso.dispatch.lock.json so later schema changes are checked for compatibility.

## Usage

```text
pso dispatch:lock [--dry-run] [--json]
```

## Flags

`--dry-run`, `--json`.

## Examples

```bash
pso dispatch:lock --dry-run
pso dispatch:lock
```

## Failure cases

- Validate declarations first; changing an existing name and version's schema requires a new event version.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [dispatch/contracts](dispatch/contracts.md)
