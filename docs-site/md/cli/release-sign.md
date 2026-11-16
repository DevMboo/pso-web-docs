# pso release:sign

> Sign the release manifest (application, immutable commit or artifact checksum, strictly increasing sequence) with a local signer. The server verifies it against its own trust store and refuses replays and downgrades

Sign the release manifest (application, immutable commit or artifact checksum, strictly increasing sequence) with a local signer. The server verifies it against its own trust store and refuses replays and downgrades.

## Usage

```text
pso release:sign <environment> --signer <name> --sequence <n> [--path <checkout>] [--artifact <file>] [--out <file>] [--json]
```

## Flags

`--signer`, `--sequence`, `--path`, `--artifact`, `--out`, `--json`.

## Examples

```bash
pso release:sign production --signer release-bot --sequence 12
pso release:sign production --signer release-bot --sequence 13 --path ../app-checkout --out signed/prod-13.json
```

## What it changes

- Writes a signed release manifest: application, immutable commit (or artifact checksum), sequence and time, signed over a domain-separated Ed25519 signature.

## Failure cases

- A checkout with uncommitted changes is refused: the server deploys the commit, not the working tree.
- A sequence not greater than the last signed release deployed is later refused by the server as a replay or downgrade.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).
