# pso frontend:graph

> Show how the FEI, its MEIs and the orchestrator relate

Show how the FEI, its MEIs and the orchestrator relate.

## Usage

```text
pso frontend:graph [--mermaid] [--json]
```

## Flags

`--mermaid`, `--json`.

## Examples

```bash
pso frontend:graph
pso frontend:graph --mermaid
```

## What it changes

- Shows how the shell, its microfrontends and the orchestrator relate. `--mermaid` prints a diagram source.

## Illustrative output

This sample shows the shape of the result. Names, ports, digests and timings may differ in your workspace.

```text evidence="illustrative"
fei-shop (:4200)
├── mei-customers (:9000)  /customers
└── shop-gateway  (orchestrator, /api)
```

## Failure cases

- Prints nothing useful without a FEI: create one with `pso create fei <name>`.
- A MEI that is not registered appears as unlinked: run `pso frontend:validate` for the reason.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [frontend/overview](frontend/overview.md)
