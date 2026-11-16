# pso release:logs

> Sanitized logs from the server: a deployment's log (default: the latest) or the active release's application log of a service. Secrets known to the server are masked

Sanitized logs from the server: a deployment's log (default: the latest) or the active release's application log of a service. Secrets known to the server are masked.

## Usage

```text
pso release:logs <environment> [--deployment <id>] [--service <name>] [--bytes 16000] [--json]
```

## Flags

`--deployment`, `--service`, `--bytes`, `--json`.

## Examples

```bash
pso release:logs production
pso release:logs production --service catalog
pso release:logs production --deployment d1abc... --bytes 40000
```

## Failure cases

- Logs are sanitized by pattern and by the secrets known to the server; an application that prints a secret in an unrecognisable form can still write it to its own log file.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [cloud/credentials-and-security](cloud/credentials-and-security.md)
