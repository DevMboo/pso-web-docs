# pso release:history

> Deployment history from the server (newest first): who, what, strategy, outcome

Deployment history from the server (newest first): who, what, strategy, outcome.

## Usage

```text
pso release:history <environment> [--limit 20] [--json]
```

## Flags

`--limit`, `--json`.

## Examples

```bash
pso release:history production --limit 10
```

## Failure cases

- History is read from the server, not stored locally; a server that cannot be reached shows an error instead of an empty list.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [cloud/rollback-and-recovery](cloud/rollback-and-recovery.md)
