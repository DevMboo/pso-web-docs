# pso connection:rotate

> Rotate credentials: by default the Ed25519 client key used with the agent (enroll a new key, revoke the old); with --ssh-secret the stored SSH password or key passphrase (new value on stdin)

Rotate credentials: by default the Ed25519 client key used with the agent (enroll a new key, revoke the old); with --ssh-secret the stored SSH password or key passphrase (new value on stdin).

## Usage

```text
pso connection:rotate <name> [--ssh-secret] [--json]
```

## Flags

`--ssh-secret`, `--json`.

## Examples

```bash
pso connection:rotate prod
pso connection:rotate prod --ssh-secret
```

## What it changes

- Without a flag: enrolls a new Ed25519 client key with a token created by your current identity, then revokes the old one and deletes its key.
- With `--ssh-secret`: replaces the stored SSH password or key passphrase (new value on standard input).

## Failure cases

- Rotation needs an enrolled connection with a session that may create enrollment tokens (the role you already hold); with an ssh-agent or an unprotected key there is no stored SSH secret to rotate and `--ssh-secret` says so.
- If the new key is enrolled but revoking the old one fails the old identity stays valid: run `pso environment:access <env> --revoke <client-id>`.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [cloud/credentials-and-security](cloud/credentials-and-security.md)
