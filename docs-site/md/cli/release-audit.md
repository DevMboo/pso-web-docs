# pso release:audit

> The server's audit log (sanitized; hash-chained, so edits are detectable by --verify). It is evidence against accidents, not against a fully privileged server administrator

The server's audit log (sanitized; hash-chained, so edits are detectable by --verify). It is evidence against accidents, not against a fully privileged server administrator.

## Usage

```text
pso release:audit <environment> [--limit 50] [--event <prefix>] [--verify] [--json]
```

## Flags

`--limit`, `--event`, `--verify`, `--json`.

## Examples

```bash
pso release:audit production --limit 20
pso release:audit production --verify
```

## Failure cases

- The chain detects edits; a fully privileged server administrator can rewrite all of it. Ship the head hash to a system they do not control for stronger guarantees.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [cloud/credentials-and-security](cloud/credentials-and-security.md)
