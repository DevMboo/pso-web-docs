# pso docker:logs

> Show a container's log (credentials, tokens and known secret values are masked)

Show a container's log (credentials, tokens and known secret values are masked).

## Usage

```text
pso docker:logs <name> [--env <name>] [--tail 200] [--since 10m] [--follow]
```

## Flags

`--env`, `--tail`, `--since`, `--follow`.

## Examples

```bash
pso docker:logs gateway
pso docker:logs catalog --tail 50 --since 10m
pso docker:logs edge --follow
```

## What it changes

- Private keys, tokens, credentials and known secret values are masked in every line.

## Failure cases

- `--since` takes a duration such as 30s, 10m or 2h.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).
