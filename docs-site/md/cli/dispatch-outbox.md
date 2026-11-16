# pso dispatch:outbox

> Enable the transactional outbox in a Spring service with PostgreSQL: adds the pso_outbox Flyway migration and sets pso.dispatch.outbox.enabled (idempotent)

Enable the transactional outbox in a Spring service with PostgreSQL: adds the pso_outbox Flyway migration and sets pso.dispatch.outbox.enabled (idempotent).

## Usage

```text
pso dispatch:outbox [--service <name>] [--dry-run] [--json]
```

## Flags

`--service`, `--dry-run`, `--json`.

## Examples

```bash
pso dispatch:outbox --service orders --dry-run
pso dispatch:outbox --service orders
```

## Failure cases

- Requires a Spring service with PostgreSQL and an installed Dispatch provider; the command does not implement a TypeScript outbox.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [dispatch/outbox](dispatch/outbox.md)
