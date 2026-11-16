# pso agent:releases

> List the agent releases kept on the server: the active and the previous one, each build's version, build id, checksum, installation time and installer, and the history of activations and rollbacks

List the agent releases kept on the server: the active and the previous one, each build's version, build id, checksum, installation time and installer, and the history of activations and rollbacks.

## Usage

```text
pso agent:releases <connection> [--json]
```

## Flags

`--json`.

## Examples

```bash
pso agent:releases prod
pso agent:releases prod --json
```

## Failure cases

- A server whose agent was installed before releases were versioned shows layout `legacy` until its next upgrade adopts the running file as a release.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [cloud/agent-and-enrollment](cloud/agent-and-enrollment.md)
