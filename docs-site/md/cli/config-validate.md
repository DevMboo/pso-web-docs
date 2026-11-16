# pso config:validate

> Validate configuration against the typed schema and secret-handling rules (exit 1 on errors; unknown keys are errors with --strict and in production)

Validate configuration against the typed schema and secret-handling rules (exit 1 on errors; unknown keys are errors with --strict and in production).

## Usage

```text
pso config:validate [--service name] [--env e] [--strict] [--json]
```

## Flags

`--service`, `--env`, `--strict`, `--json`.

## Examples

```bash
pso config:validate
pso config:validate --service gateway --env production
```

## Failure cases

- `PSO_CONFIG_PLAINTEXT_SECRET`: store secrets as references such as `${env:NAME}`.
- `PSO_CONFIG_INVALID_VALUE`: a value does not match the typed schema.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).
