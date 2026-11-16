# pso environment:plan

> Show what provisioning would do on the server: steps PSO performs unprivileged, steps an administrator must run, and the exact script for them (nothing is executed)

Show what provisioning would do on the server: steps PSO performs unprivileged, steps an administrator must run, and the exact script for them (nothing is executed).

## Usage

```text
pso environment:plan <environment> [--json]
```

## Flags

`--json`.

## Examples

```bash
pso environment:plan production
```

## What it changes

- Lists what provisioning would do: unprivileged steps PSO performs, steps an administrator must run, and the exact script. Nothing is executed.

## Failure cases

- Nothing is executed: the administrator script is for you to read and run. If the agent cannot run nginx -t the plan says which step is missing.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [cloud/nginx-and-existing-infrastructure](cloud/nginx-and-existing-infrastructure.md)
