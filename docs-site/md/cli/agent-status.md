# pso agent:status

> Show the agent's version, mode and who is enrolled

Show the agent's version, mode and who is enrolled.

## Usage

```text
pso agent:status <connection> [--json]
```

## Flags

`--json`.

## Examples

```bash
pso agent:status prod
pso agent:status prod --json
```

## Failure cases

- The client list needs the admin role; with a lower role the command still shows the agent version, mode and identity, and says the list is unavailable.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [cloud/agent-and-enrollment](cloud/agent-and-enrollment.md)
