# pso make:migration

> Create the next Flyway migration (V<n>__<name>.sql) in a PostgreSQL-backed service

Create the next Flyway migration (V<n>__<name>.sql) in a PostgreSQL-backed service.

## Usage

```text
pso make:migration <name> --service <name> [--dry-run] [--json]
```

## Flags

`--service`, `--dry-run`, `--json`.

## Examples

```bash
pso make:migration add-orders --service orders
```

## Failure cases

- `has no database`: create the service with `--db postgres`.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [microservices/persistence](microservices/persistence.md)
