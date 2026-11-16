# pso environment:secret

> Store a named secret on the server (value from standard input or a hidden prompt) for services to receive as environment variables; --list shows names only, never values

Store a named secret on the server (value from standard input or a hidden prompt) for services to receive as environment variables; --list shows names only, never values.

## Usage

```text
pso environment:secret <environment> <name> [--remove] [--list] [--json]
```

## Flags

`--remove`, `--list`, `--json`.

## Examples

```bash
pso environment:secret production shop-db-password
pso environment:secret production --list
pso environment:secret production shop-db-password --remove
```

## What it changes

- Stores the value (standard input or a hidden prompt) on the server with mode 600; services receive it as an environment variable named in their declaration.

## Failure cases

- Values are never shown again; `--list` prints names only.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [cloud/credentials-and-security](cloud/credentials-and-security.md)
