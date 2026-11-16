# pso config:show

> Show the effective configuration with the origin of every value (secrets redacted)

Show the effective configuration with the origin of every value (secrets redacted).

## Usage

```text
pso config:show [--service name] [--env e] [--set key=value ...] [--json]
```

## Flags

| Flag | What it does |
|---|---|
| `--service` | Select the registered service whose configuration is resolved. |
| `--env` | Choose development, test, staging or production. |
| `--set` | Apply a temporary command-line override for this invocation. |
| `--json` | Print machine-readable values and origins. |

## Examples

```bash
pso config:show --service gateway --env production
pso config:show --set logging.level=debug
```

## What it changes

- Resolves configuration layers for one service and environment without changing stored files.
- Secret-looking values are redacted in the output.

## Verify the result

- Read the source shown for each effective value, then run `pso config:validate` for schema errors.
- Start the service to verify settings that depend on a live key, database or broker.

## Failure cases

- `Unknown environment`: use development, test, staging or production.
- Secret-looking keys are always printed as `***REDACTED***`.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [concepts/configuration](concepts/configuration.md)
