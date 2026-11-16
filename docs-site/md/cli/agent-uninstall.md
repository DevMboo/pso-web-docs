# pso agent:uninstall

> Remove the management agent from the server. Applications, their data and processes stay unless --purge-applications is given (refused while services run)

Remove the management agent from the server. Applications, their data and processes stay unless --purge-applications is given (refused while services run).

## Usage

```text
pso agent:uninstall <connection> --yes [--purge-applications] [--json]
```

## Flags

`--yes`, `--purge-applications`, `--json`.

## Examples

```bash
pso agent:uninstall prod --yes
```

## Failure cases

- Applications, their data and processes are left alone; `--purge-applications` is refused while services are running.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [cloud/agent-and-enrollment](cloud/agent-and-enrollment.md)
