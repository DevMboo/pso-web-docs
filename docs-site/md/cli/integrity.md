# pso integrity

> Verify the installed files against the signed inventory (read-only: nothing is ever modified)

Verify the installed files against the signed inventory (read-only: nothing is ever modified).

## Usage

```text
pso integrity check [--full] [--root <dir>] [--trust <public.pem>] [--json] | report [--full] [--root <dir>] [--json]
```

## Flags

`--full`, `--root`, `--trust`, `--json`.

## Examples

```bash
pso integrity check --root C:Toolspso
pso integrity check --full --root C:Toolspso
pso integrity report --json --root C:Toolspso
```

## Failure cases

- Read-only. A quick check compares presence and size; only --full proves content with SHA-256. Exit 1 means problems were found; run `pso repair`.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).
