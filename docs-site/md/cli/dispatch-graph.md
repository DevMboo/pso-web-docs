# pso dispatch:graph

> Show the declared event topology: publishers -> channels -> consumers (declarations, not live broker state)

Show the declared event topology: publishers -> channels -> consumers (declarations, not live broker state).

## Usage

```text
pso dispatch:graph [--mermaid] [--json]
```

## Flags

`--mermaid`, `--json`.

## Examples

```bash
pso dispatch:graph
pso dispatch:graph --mermaid
```

## Failure cases

- The topology is declared state; it is not a live throughput or lag view.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [dispatch/visualization](dispatch/visualization.md)
