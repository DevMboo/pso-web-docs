# pso connection:revoke

> Revoke this machine's access: the server forgets the client identity and every local secret of the connection is deleted

Revoke this machine's access: the server forgets the client identity and every local secret of the connection is deleted.

## Usage

```text
pso connection:revoke <name> --yes [--json]
```

## Flags

`--yes`, `--json`.

## Examples

```bash
pso connection:revoke prod --yes
```

## Failure cases

- Exit code 3 without `--yes`.
- When the server cannot be reached the local secrets are still deleted and the command says the server was NOT reached: revoke the client from another administrator.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [cloud/credentials-and-security](cloud/credentials-and-security.md)
