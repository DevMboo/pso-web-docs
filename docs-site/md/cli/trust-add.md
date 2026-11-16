# pso trust:add

> Make the environment trust a signer's PUBLIC key (superadministrator, audited). A server that requires signed releases accepts only signers added here; no project file can add one

Make the environment trust a signer's PUBLIC key (superadministrator, audited). A server that requires signed releases accepts only signers added here; no project file can add one.

## Usage

```text
pso trust:add <environment> (--signer <name> | --public-key-file <file> --name <name>) [--apps a,b] [--not-after <date>] --yes [--json]
```

## Flags

`--signer`, `--public-key-file`, `--name`, `--apps`, `--not-after`, `--yes`, `--json`.

## Examples

```bash
pso trust:add production --signer release-bot --yes
pso trust:add production --public-key-file ci.pub.pem --name ci --apps shop-catalog --not-after 2027-06-01 --yes
```

## What it changes

- The environment's server will accept releases signed by that public key (for the listed applications, until the date).

## Failure cases

- Superadministrator only. A revoked key can never be added again; generate a new one.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).
