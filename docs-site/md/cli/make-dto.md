# pso make:dto

> Generate a dto (orchestrator: inside a module; Spring: hexagonal layout)

Generate a dto (orchestrator: inside a module; Spring: hexagonal layout).

## Usage

```text
pso make:dto <module> <name> [--response] [--fields "name:string:min=3,email:email,age?:integer:min=18,document:file:mime=application/pdf:size=10MB"] [--service <orchestrator>] [--dry-run] [--json] (Spring: pso make:dto <name> --service <svc> [--dry-run] [--force] [--json])
```

## Flags

`--response`, `--fields`, `--service`, `--dry-run`, `--json`, `--force`.

## Examples

```bash
pso make:dto users create-user
```

## Failure cases

- A TypeScript DTO type does not validate the incoming JSON at runtime; add explicit checks or delegate to Spring validation.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [orchestrator/dtos](orchestrator/dtos.md)
