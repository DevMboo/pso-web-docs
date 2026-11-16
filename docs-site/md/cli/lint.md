# pso lint

> Static checks: workspace validation, configuration, security check (for one environment, with the sources the application runs with in it) and TypeScript type-check

Static checks: workspace validation, configuration, security check (for one environment, with the sources the application runs with in it) and TypeScript type-check.

## Usage

```text
pso lint [--env development|test|staging|production] [--json]
```

## Flags

`--env`, `--json`.

## Examples

```bash
pso lint
pso lint --env production
```

## What it changes

- Runs the workspace validation, the security check for one environment (default: PSO_ENV, else development, with the configuration sources the application runs with in it) and the TypeScript type-check.

## Failure cases

- Reports (exit 1) when dependencies are missing instead of silently skipping the type-check.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).
