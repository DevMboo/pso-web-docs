# pso frontend:validate

> Validate the FEI/MEI declarations, their relationships, route ownership and conflicts, contract compatibility and the architecture rules (exit 1 on errors)

Validate the FEI/MEI declarations, their relationships, route ownership and conflicts, contract compatibility and the architecture rules (exit 1 on errors).

## Usage

```text
pso frontend:validate [--json]
```

## Flags

`--json`.

## Examples

```bash
pso frontend:validate
pso frontend:validate --json
```

## What it changes

- Reads the declarations without executing them and checks relationships, route ownership and conflicts, contract compatibility, orchestrator contract freshness and the architecture rules (no router inside a MEI, no absolute URL in fetch). Also part of `pso validate`.

## Illustrative output

This sample shows the shape of the result. Names, ports, digests and timings may differ in your workspace.

```text evidence="illustrative"
fei-commerce: 3 microfrontend project(s), 10 route(s)
OK
```

## Failure cases

- Exit code 1 with a diagnostic that names the file and the configuration responsible, for example `PSO_FEI_ROUTE_CONFLICT`.
- Warnings do not change the exit code.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [frontend/declarations](frontend/declarations.md)
- [frontend/troubleshooting](frontend/troubleshooting.md)
