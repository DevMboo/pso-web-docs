# pso make:contract

> Generate a service contract (JSON) in src/contracts/ (orchestrator): the operations the orchestrator expects from a service

Generate a service contract (JSON) in src/contracts/ (orchestrator): the operations the orchestrator expects from a service.

## Usage

```text
pso make:contract <name> [--service <name>] [--route /path] [--method GET] [--dry-run] [--force] [--json]
```

## Flags

`--service`, `--route`, `--method`, `--dry-run`, `--force`, `--json`.

## Examples

```bash
pso make:contract order --service catalog --dry-run
pso make:contract order --service catalog
```

## Failure cases

- Review the generated contract against both sides of the integration; a stub is not an implemented endpoint.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [concepts/contracts](concepts/contracts.md)
