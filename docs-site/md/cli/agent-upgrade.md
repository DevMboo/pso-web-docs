# pso agent:upgrade

> Install the agent bundled in this installation as a new versioned release on the server: verified upload, start check, one atomic link switch; every earlier release is kept (see agent:releases) and can be returned to with agent:rollback. Identity, roles, policy and deployments are untouched; the policy is never changed by an upgrade

Install the agent bundled in this installation as a new versioned release on the server: verified upload, start check, one atomic link switch; every earlier release is kept (see agent:releases) and can be returned to with agent:rollback. Identity, roles, policy and deployments are untouched; the policy is never changed by an upgrade.

## Usage

```text
pso agent:upgrade <connection> --yes [--json]
```

## Flags

`--yes`, `--json`.

## Examples

```bash
pso agent:upgrade prod --yes
```

## What it changes

- Uploads the bundled agent, verifies the checksum, starts it once as a check, swaps it atomically and keeps the previous file.

## Failure cases

- The agent never updates itself; exit code 3 without `--yes`.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [cloud/agent-and-enrollment](cloud/agent-and-enrollment.md)
