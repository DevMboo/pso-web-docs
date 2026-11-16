# pso make:filter

> Generate an error filter in src/filters/ (orchestrator). It runs only for the routes that name it in ROUTES.pso.ts

Generate an error filter in src/filters/ (orchestrator). It runs only for the routes that name it in ROUTES.pso.ts.

## Usage

```text
pso make:filter <name> [--service <name>] [--dry-run] [--force] [--json]
```

## Flags

`--service`, `--dry-run`, `--force`, `--json`.

## Examples

```bash
pso make:filter request-log --service gateway --dry-run
pso make:filter request-log --service gateway
```

## Failure cases

- A generated file is not applied to a route until you declare it in the route registry.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [orchestrator/middleware-and-filters](orchestrator/middleware-and-filters.md)
