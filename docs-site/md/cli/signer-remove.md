# pso signer:remove

> Delete a signing key from THIS machine. It does not revoke trust on servers: if the key may be compromised, run pso trust:revoke on every environment first

Delete a signing key from THIS machine. It does not revoke trust on servers: if the key may be compromised, run pso trust:revoke on every environment first.

## Usage

```text
pso signer:remove <name> --yes [--json]
```

## Flags

`--yes`, `--json`.

## Examples

```bash
pso signer:remove release-bot --yes
```

## What it changes

- Deletes the private key from this machine's credential store.

## Failure cases

- Does not revoke trust on any server: run `pso trust:revoke` on every environment first when the key may be compromised.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).
