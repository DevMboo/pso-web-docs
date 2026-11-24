# pso make:mapper

> Generate a mapper. Orchestrator (modular): a mapping function in modules/<module>/mappers/; Spring: a mapper component (layered: application/mapper)

Generate a mapper. Orchestrator (modular): a mapping function in modules/<module>/mappers/; Spring: a mapper component (layered: application/mapper).

## Usage

```text
pso make:mapper <module> <name> [--service <orchestrator>] [--dry-run] [--json] (Spring: pso make:mapper <name> --service <svc> [--dry-run] [--force] [--json])
```

## Flags

`--service`, `--dry-run`, `--json`, `--force`.

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
