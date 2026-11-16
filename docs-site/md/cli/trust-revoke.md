# pso trust:revoke

> Revoke a trusted signer (superadministrator, audited). A revoked key is never trusted again; releases it signed no longer qualify, including for rollback where signing is required

Revoke a trusted signer (superadministrator, audited). A revoked key is never trusted again; releases it signed no longer qualify, including for rollback where signing is required.

## Usage

```text
pso trust:revoke <environment> <key-id> [--reason <text>] --yes [--json]
```

## Flags

`--reason`, `--yes`, `--json`.

## Examples

```bash
pso trust:revoke production SHA256:abc --reason "key leaked" --yes
```

## What it changes

- Releases signed by that key stop qualifying immediately, including at start time of an already approved plan and for rollback where signing is required.

## Failure cases

- Superadministrator only.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).
