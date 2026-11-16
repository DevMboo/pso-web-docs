# pso make:mapper

> Generate a mapper component (Spring, hexagonal layout: adapters/)

Generate a mapper component (Spring, hexagonal layout: adapters/).

## Usage

```text
pso make:mapper <name> [--service <name>] [--dry-run] [--force] [--json]
```

## Flags

`--service`, `--dry-run`, `--force`, `--json`.

## Examples

```bash
pso make:mapper product --service catalog --dry-run
pso make:mapper product --service catalog
```

## Failure cases

- Generated mapping code is a stub; check field conversion and null handling in service tests.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [microservices/hexagonal-architecture](microservices/hexagonal-architecture.md)
