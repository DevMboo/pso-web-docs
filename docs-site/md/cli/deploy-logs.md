# pso deploy:logs

> Show a container's log from the environment's host (secrets, tokens and keys masked)

Show a container's log from the environment's host (secrets, tokens and keys masked).

## Usage

```text
pso deploy:logs <service> --env <name> [--tail 200]
```

## Flags

`--env`, `--tail`.

## Examples

```bash
pso deploy:logs gateway --env staging
pso deploy:logs catalog --env staging --tail 200
```

## What it changes

- The last lines of one container of the active release on the host; known secret values and key material are masked.

## Failure cases

- `Name the container`: `pso deploy:status --env <name>` lists them.
- A local environment: the hint names the `pso docker:logs` to run instead.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [infrastructure/remote-hosts](infrastructure/remote-hosts.md)
