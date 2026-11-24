# pso make:response

> Generate a response record in application/dto/response (layered Spring service)

Generate a response record in application/dto/response (layered Spring service).

## Usage

```text
pso make:response <name> [--service <name>] [--dry-run] [--force] [--json]
```

## Flags

`--service`, `--dry-run`, `--force`, `--json`.

## Examples

```bash
pso make:response supplier --service catalog
```

## What it changes

- Writes a response record in `application/dto/response`.

## Failure cases

- `needs the layered structure`: 0.1 services use `pso make:dto`.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [architecture/java-microservice](architecture/java-microservice.md)
