# pso security:rotate

> Create a new active key; the previous one stays in the JWKS as 'retiring' until retired

Create a new active key; the previous one stays in the JWKS as 'retiring' until retired.

## Usage

```text
pso security:rotate [--retire <kid>] [--alg ES256|RS256] [--out keys] [--dry-run] [--json]
```

## Flags

`--retire`, `--alg`, `--out`, `--dry-run`, `--json`.

## Examples

```bash
pso security:rotate
pso security:rotate --retire k-20261107-fd3cc326
```

## Failure cases

- The active key cannot be retired.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [security/keys-and-pem](security/keys-and-pem.md)
