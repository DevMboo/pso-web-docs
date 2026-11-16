# pso dispatch:install

> Install a dispatch provider into a Spring service or the orchestrator (dependency, configuration, local broker file). Dispatch stays disabled until you enable it

Install a dispatch provider into a Spring service or the orchestrator (dependency, configuration, local broker file). Dispatch stays disabled until you enable it.

## Usage

```text
pso dispatch:install <redis|rabbitmq|kafka> [--service <name>] [--dry-run] [--json]
```

## Flags

| Flag | What it does |
|---|---|
| `--service` | Choose the service that owns this Dispatch adapter. |
| `--dry-run` | Preview the dependency and configuration changes. |
| `--json` | Print machine-readable results. |

## Examples

```bash
pso dispatch:install redis --service orders --dry-run
pso dispatch:install redis --service orders
```

## What it changes

- Adds the selected adapter dependency and service configuration; a Redis install also writes local broker guidance and a Compose descriptor.
- Dispatch remains disabled until you declare an event and enable the provider deliberately.

## Verify the result

- Run `pso dispatch:inspect` to see the installed provider and disabled state.
- After declaring events, run `pso dispatch:validate`; use `pso dispatch:doctor` only when a broker is running.

## Failure cases

- A second provider on the same service is refused; remove the first deliberately. Installation leaves dispatch disabled.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [dispatch/installation](dispatch/installation.md)
