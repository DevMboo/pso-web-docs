# pso make:adapter

> Generate an adapter component (Spring, hexagonal layout: adapters/)

Generate an adapter component (Spring, hexagonal layout: adapters/).

## Usage

```text
pso make:adapter <name> [--service <name>] [--dry-run] [--force] [--json]
```

## Flags

`--service`, `--dry-run`, `--force`, `--json`.

## Examples

```bash
pso make:adapter postgres --service catalog --dry-run
pso make:adapter postgres --service catalog
```

## Failure cases

- The selected runtime determines the generated file path; inspect the dry run and edit the stub for your application.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [microservices/hexagonal-architecture](microservices/hexagonal-architecture.md)
