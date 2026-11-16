# pso studio

> Open the local developer dashboard (live services, graph, routes, config, security, logs)

Open the local developer dashboard (live services, graph, routes, config, security, logs).

## Usage

```text
pso studio [--port N] [--no-open] [--remote] [--frontend-ops]
```

## Flags

| Flag | What it does |
|---|---|
| `--port` | Choose the local Studio port. |
| `--no-open` | Print the URL without launching a browser. |
| `--remote` | See the usage line and the task guide for this option. |
| `--frontend-ops` | See the usage line and the task guide for this option. |

## Examples

```bash
pso studio
pso studio --no-open
```

## What it changes

- Starts a loopback-only, read-only dashboard for the current workspace; the URL carries a one-time token.
- Views combine declarations, health probes and logs and say when a live source is unavailable.

## Verify the result

- Open the printed URL and compare Overview route counts with `pso routes:list`.
- Compare service states with `pso services:health`; registration alone is not health.

## Failure cases

- Read-only and loopback-only; the printed URL contains a one-time token.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).
