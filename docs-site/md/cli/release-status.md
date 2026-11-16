# pso release:status

> Active, previous and last known-good release, the lock, the services and the latest (or chosen) deployment as the server sees it (INTERRUPTED when its worker is gone)

Active, previous and last known-good release, the lock, the services and the latest (or chosen) deployment as the server sees it (INTERRUPTED when its worker is gone).

## Usage

```text
pso release:status <environment> [--deployment <id>] [--json]
```

## Flags

`--deployment`, `--json`.

## Examples

```bash
pso release:status production
pso release:status production --deployment d1abc...
```

## Failure cases

- `INTERRUPTED` means the worker on the server is gone; see `release:recover`.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [cloud/rollback-and-recovery](cloud/rollback-and-recovery.md)
