# pso docker:inspect

> Show the effective definition of a container (image, environment, mounts, secrets, health check, limits, dependencies) as Docker Compose resolves it, with its runtime state

Show the effective definition of a container (image, environment, mounts, secrets, health check, limits, dependencies) as Docker Compose resolves it, with its runtime state.

## Usage

```text
pso docker:inspect [<name>] [--env <name>] [--json]
```

## Flags

`--env`, `--json`.

## Examples

```bash
pso docker:inspect
pso docker:inspect gateway --json
```

## Failure cases

- Docker must be running: the definition is the one Compose resolves.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).
