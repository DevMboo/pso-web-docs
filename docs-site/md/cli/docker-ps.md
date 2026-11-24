# pso docker:ps

> List the environment's containers with their state and health

List the environment's containers with their state and health.

## Usage

```text
pso docker:ps [--env <name>] [--json]
```

## Flags

`--env`, `--json`.

## Examples

```bash
pso docker:ps
pso docker:ps --env local --json
```

## Failure cases

- The compose file must have been generated (`pso infra:apply`).

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).
