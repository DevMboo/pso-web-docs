# pso make:guard

> Generate a guard (orchestrator: inside a module; Spring: hexagonal layout)

Generate a guard (orchestrator: inside a module; Spring: hexagonal layout).

## Usage

```text
pso make:guard <name> | <module> <name> [--service <name>] [--dry-run] [--force] [--json] (application-wide in src/.app/guards/ (0.1: src/guards/); the module form creates <module>/guards/<name>.guard.ts)
```

## Flags

`--service`, `--dry-run`, `--force`, `--json`.

## Examples

```bash
pso make:guard authenticated
pso make:guard users users-key
```

## Failure cases

- The generated guard denies by default. Replace its body with a verified policy before attaching it to a route.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [orchestrator/guards](orchestrator/guards.md)
