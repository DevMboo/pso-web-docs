# pso make:client

> Generate an outbound client in infrastructure/client (layered Spring service) that calls a declared service through PsoHttpClient

Generate an outbound client in infrastructure/client (layered Spring service) that calls a declared service through PsoHttpClient.

## Usage

```text
pso make:client <name> [--service <name>] [--dry-run] [--force] [--json]
```

## Flags

`--service`, `--dry-run`, `--force`, `--json`.

## Examples

```bash
pso make:client pricing --service catalog
```

## What it changes

- Writes an outbound client in `infrastructure/client` that calls the declared service through `PsoHttpClient`.

## Failure cases

- `needs the layered structure`: see `pso structure:inspect`.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [architecture/java-microservice](architecture/java-microservice.md)
