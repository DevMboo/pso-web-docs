# pso docker:down

> Stop and remove the environment's containers and networks. Persistent data (database volumes) is kept; --volumes deletes it too and needs --yes

Stop and remove the environment's containers and networks. Persistent data (database volumes) is kept; --volumes deletes it too and needs --yes.

## Usage

```text
pso docker:down [--env <name>] [--volumes] [--yes] [--json]
```

## Flags

`--env`, `--volumes`, `--yes`, `--json`.

## Examples

```bash
pso docker:down
pso docker:down --env local --volumes --yes
```

## What it changes

- Stops and removes containers and networks; database volumes are kept unless `--volumes --yes`.

## Failure cases

- Exit code 3 for `--volumes` without `--yes`: the volumes to delete are listed.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).
