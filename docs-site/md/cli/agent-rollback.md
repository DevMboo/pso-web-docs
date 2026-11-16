# pso agent:rollback

> Return the server's agent to an earlier release (the previous one, or a named one from agent:releases): the release's checksum is verified again and it must start before the link is switched atomically. Server identity, roles, policy and deployments are untouched

Return the server's agent to an earlier release (the previous one, or a named one from agent:releases): the release's checksum is verified again and it must start before the link is switched atomically. Server identity, roles, policy and deployments are untouched.

## Usage

```text
pso agent:rollback <connection> [--to <release id>] --yes [--json]
```

## Flags

`--to`, `--yes`, `--json`.

## Examples

```bash
pso agent:rollback prod --yes
pso agent:rollback prod --to 0.2.0+3f9a1c2b7d10 --yes
```

## What it changes

- Switches the link of the running agent to an earlier release after verifying its recorded checksum and starting it once; identity, roles, policy and deployments are untouched.

## Failure cases

- A release whose bytes changed on disk, that does not start, or that is already the active one is refused and nothing changes.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [cloud/agent-and-enrollment](cloud/agent-and-enrollment.md)
