# pso services:graph

> Show the service dependency graph and startup order

Show the service dependency graph and startup order.

## Usage

```text
pso services:graph [--mermaid] [--json]
```

## Flags

`--mermaid`, `--json`.

## Examples

```bash
pso services:graph
pso services:graph --mermaid
```

## Failure cases

- The graph uses declared dependencies and startup order, not observed network calls.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [microservices/service-registration](microservices/service-registration.md)
