# pso make:port

> Generate a port interface (Spring, 0.1 hexagonal layout only: ports/; the layered structure has no ports)

Generate a port interface (Spring, 0.1 hexagonal layout only: ports/; the layered structure has no ports).

## Usage

```text
pso make:port <name> [--service <name>] [--dry-run] [--force] [--json]
```

## Flags

`--service`, `--dry-run`, `--force`, `--json`.

## Examples

```bash
pso make:port payment --service orders --dry-run
pso make:port payment --service orders
```

## Failure cases

- The generated port is an application contract; provide an adapter implementation before using it.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [microservices/hexagonal-architecture](microservices/hexagonal-architecture.md)
