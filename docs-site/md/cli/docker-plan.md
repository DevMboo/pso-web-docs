# pso docker:plan

> Show the container topology of an environment before anything runs: startup order, images built or pulled, published ports, networks, volumes, secrets (names only), health checks and resource limits

Show the container topology of an environment before anything runs: startup order, images built or pulled, published ports, networks, volumes, secrets (names only), health checks and resource limits.

## Usage

```text
pso docker:plan [--env <name>] [--json]
```

## Flags

`--env`, `--json`.

## Examples

```bash
pso docker:plan
pso docker:plan --env staging --json
```

## What it changes

- Prints the container topology in startup order with images, published ports, networks, volumes, secrets (names) and pending infrastructure changes. Needs no Docker.

## Failure cases

- The same contract errors as `pso infra:plan`.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [infrastructure/containers](infrastructure/containers.md)
