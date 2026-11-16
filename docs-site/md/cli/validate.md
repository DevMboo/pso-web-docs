# pso validate

> Validate workspace structure, conventions and contracts (exit 1 on errors)

Validate workspace structure, conventions and contracts (exit 1 on errors).

## Usage

```text
pso validate [--write-manifest [--dry-run]] [--json]
```

## Flags

| Flag | What it does |
|---|---|
| `--write-manifest` | Persist the deterministic discovery manifest after validation. |
| `--dry-run` | See the usage line and the task guide for this option. |
| `--json` | Return diagnostics in machine-readable form. |

## Examples

```bash
pso validate
pso validate --json
pso validate --write-manifest
```

## What it changes

- Reads workspace structure, declarations and contracts without starting the application.
- Only `--write-manifest` writes a manifest; ordinary validation leaves project files unchanged.

## Illustrative output

This sample shows the shape of the result. Names, ports, digests and timings may differ in your workspace.

```text evidence="illustrative"
gateway: 5 resource(s), digest 618af77aaa00
catalog: 4 resource(s), digest a29b9b85d072
OK
```

## Verify the result

- An `OK` and exit code 0 mean the static checks passed.
- Use `pso services:health` and an HTTP request for runtime evidence.

## Failure cases

- Exit code 1 with diagnostics such as `PSO_ROUTE_004` (method mismatch) or `PSO_PORT_CONFLICT`. Nothing is executed to produce them.
- `No pso.workspace.json found`: run it inside a workspace.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [orchestrator/routes](orchestrator/routes.md)
- [reference/diagnostics](reference/diagnostics.md)
