# pso security:keys

> List signing keys (public data only) or generate the first keypair

List signing keys (public data only) or generate the first keypair.

## Usage

```text
pso security:keys [--generate] [--alg ES256|RS256] [--out keys] [--json]
```

## Flags

`--generate`, `--alg`, `--out`, `--json`.

## Examples

```bash
pso security:keys --generate --alg ES256
pso security:keys
```

## Failure cases

- Refuses to generate while an active key exists (use `security:rotate`).
- Private key material is never printed.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [security/keys-and-pem](security/keys-and-pem.md)
