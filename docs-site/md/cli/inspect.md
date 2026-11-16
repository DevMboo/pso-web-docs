# pso inspect

> List the resources PSO discovered in a project

List the resources PSO discovered in a project.

## Usage

```text
pso inspect [--service <name>] [--json]
```

## Flags

`--service`, `--json`.

## Examples

```bash
pso inspect --service gateway
pso inspect --service catalog --json
```

## Failure cases

- The named service must be registered in the workspace; discovery describes files, not a running process.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [concepts/structural-discovery](concepts/structural-discovery.md)
