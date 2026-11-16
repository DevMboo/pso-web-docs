# pso environment:access

> List who may manage the environment; --invite creates a single-use enrollment token for a teammate (shown once), --revoke ends a client's access immediately

List who may manage the environment; --invite creates a single-use enrollment token for a teammate (shown once), --revoke ends a client's access immediately.

## Usage

```text
pso environment:access <environment> [--invite viewer|operator|deployer|admin] [--minutes 15] [--revoke <client-id>] [--json]
```

## Flags

`--invite`, `--minutes`, `--revoke`, `--json`.

## Examples

```bash
pso environment:access production
pso environment:access production --invite deployer --minutes 15
pso environment:access production --revoke SHA256:AbCd...
```

## Failure cases

- An invitation cannot grant a role above your own; the last superadministrator cannot be revoked.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [cloud/agent-and-enrollment](cloud/agent-and-enrollment.md)
