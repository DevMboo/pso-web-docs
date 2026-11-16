# pso connection:test

> Check a connection layer by layer: SSH and host identity, the agent, the agent's pinned identity, your authenticated session

Check a connection layer by layer: SSH and host identity, the agent, the agent's pinned identity, your authenticated session.

## Usage

```text
pso connection:test <name> [--pin-agent <SHA256:fingerprint>] [--json]
```

## Flags

`--pin-agent`, `--json`.

## Examples

```bash
pso connection:test prod
pso connection:test prod --json
```

## What it changes

- Checks SSH reachability and host identity, the agent, the agent's pinned identity and your authenticated session, and reports each layer.

## Failure cases

- `AGENT IDENTITY CHANGED`: the server presented another agent key than the pinned one; nothing was sent.
- A changed SSH host key is a hard failure, never a prompt.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [cloud/connect-a-server](cloud/connect-a-server.md)
