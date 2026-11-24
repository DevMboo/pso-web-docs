# pso docker:restart

> Restart only the named containers (their dependencies keep running). --recreate re-creates them from the current topology, which picks up changed environment, mounts or images

Restart only the named containers (their dependencies keep running). --recreate re-creates them from the current topology, which picks up changed environment, mounts or images.

## Usage

```text
pso docker:restart <name...> [--env <name>] [--recreate] [--json]
```

## Flags

`--env`, `--recreate`, `--json`.

## Examples

```bash
pso docker:restart edge
pso docker:restart catalog accounts --env local
pso docker:restart idp --recreate
```

## What it changes

- Restarts only the named containers; `--recreate` re-creates them from the current topology.

## Failure cases

- `Unknown container`: the message lists the valid names.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).
