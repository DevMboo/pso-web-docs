# pso release:service

> Inspect, restart, start or stop ONE service of the active release without touching the others (not allowed while a deployment is running)

Inspect, restart, start or stop ONE service of the active release without touching the others (not allowed while a deployment is running).

## Usage

```text
pso release:service <environment> <status|restart|start|stop> [<service>] [--yes] [--json]
```

## Flags

`--yes`, `--json`.

## Examples

```bash
pso release:service production status
pso release:service production restart catalog --yes
```

## Failure cases

- Not allowed while a deployment holds the lock; only processes started by PSO and verified by identity token are signalled.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [cloud/deploy-a-release](cloud/deploy-a-release.md)
