# pso signer:generate

> Create an Ed25519 release-signing key on THIS machine. The private key goes to the protected credential store (never to a file, a project or a server); the public key is what an environment is told to trust

Create an Ed25519 release-signing key on THIS machine. The private key goes to the protected credential store (never to a file, a project or a server); the public key is what an environment is told to trust.

## Usage

```text
pso signer:generate <name> [--json]
```

## Flags

`--json`.

## Examples

```bash
pso signer:generate release-bot
```

## What it changes

- Creates an Ed25519 key pair. The private key goes to the protected credential store of THIS machine; nothing is written to the project or to a server.

## Failure cases

- Fails when no protected credential store is available (PSO never falls back to a plain-text file).
- A name that already exists is refused: rotation means a new key, never an overwrite.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).
