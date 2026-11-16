# pso make:service

> Generate a service (orchestrator: inside a module; Spring: hexagonal layout)

Generate a service (orchestrator: inside a module; Spring: hexagonal layout).

## Usage

```text
pso make:service <module> [name] [--export] [--service <orchestrator>] [--dry-run] [--json] (Spring: pso make:service <name> --service <svc> [--dry-run] [--force] [--json])
```

## Flags

`--export`, `--service`, `--dry-run`, `--json`, `--force`.

## Examples

```bash
pso make:service users --export
pso make:service orders --service catalog   # Spring Boot form
```

## Failure cases

- A generated service is a stub; register its provider in the module and implement domain behavior before use.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [orchestrator/services-and-providers](orchestrator/services-and-providers.md)
