# pso make:entity

> Generate a JPA entity in infrastructure/entity (layered Spring service); add its table with pso make:migration

Generate a JPA entity in infrastructure/entity (layered Spring service); add its table with pso make:migration.

## Usage

```text
pso make:entity <name> [--service <name>] [--dry-run] [--force] [--json]
```

## Flags

`--service`, `--dry-run`, `--force`, `--json`.

## Examples

```bash
pso make:entity supplier --service catalog
pso make:migration create-suppliers --service catalog
```

## What it changes

- Writes a JPA entity in `infrastructure/entity`. Hibernate only validates the schema: add its table with `pso make:migration` before starting the service.

## Failure cases

- `needs a database`: the service has no JPA dependency; run `pso database:enable postgresql --service <svc>` first.
- `needs the layered structure`: 0.1 services keep their entities in `persistence/`.
- The service refuses to start (`Schema-validation: missing table`) until the migration exists.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [architecture/java-microservice](architecture/java-microservice.md)
