# pso make:filter

> Generate an error filter. Orchestrator: src/.app/filters/ (0.1: src/filters/), run for the routes that name it; Spring (layered): a servlet filter in infrastructure/filter

Generate an error filter. Orchestrator: src/.app/filters/ (0.1: src/filters/), run for the routes that name it; Spring (layered): a servlet filter in infrastructure/filter.

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
