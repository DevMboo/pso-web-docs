# pso make:request

> Generate a request record with Jakarta Validation in application/dto/request (layered Spring service)

Generate a request record with Jakarta Validation in application/dto/request (layered Spring service).

## Usage

```text
pso make:request <name> [--service <name>] [--dry-run] [--force] [--json]
```

## Flags

`--service`, `--dry-run`, `--force`, `--json`.

## Examples

```bash
pso make:request create-supplier --service catalog
```

## What it changes

- Writes a request record with Jakarta Validation in `application/dto/request`.

## Failure cases

- `needs the layered structure`: 0.1 services use `pso make:dto`.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [architecture/java-microservice](architecture/java-microservice.md)
