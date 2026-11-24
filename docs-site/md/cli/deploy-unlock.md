# pso deploy:unlock

> Remove the deployment lock an interrupted deployment left on the environment's host (only when no deployment is running)

Remove the deployment lock an interrupted deployment left on the environment's host (only when no deployment is running).

## Usage

```text
pso deploy:unlock --env <name> --yes [--json]
```

## Flags

`--env`, `--yes`, `--json`.

## Examples

```bash
pso deploy:unlock --env staging --yes
```

## What it changes

- Removes the deployment lock an interrupted deployment left on the host (and the local one). Use it only after making sure no deployment of the environment is running.

## Failure cases

- Exit 3 without `--yes`.
- `the lock could not be removed`: it belongs to another user on the host; nothing was reported as done.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [infrastructure/remote-hosts](infrastructure/remote-hosts.md)
