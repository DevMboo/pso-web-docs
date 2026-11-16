# pso signer:list

> List the signing identities of this machine (names, key ids, public keys; never private keys)

List the signing identities of this machine (names, key ids, public keys; never private keys).

## Usage

```text
pso signer:list [--json]
```

## Flags

`--json`.

## Examples

```bash
pso signer:list
pso signer:list --json
```

## Failure cases

- Lists names, key ids and public keys only; a private key is never shown.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).
