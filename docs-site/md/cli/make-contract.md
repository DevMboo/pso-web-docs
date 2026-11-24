# pso make:contract

> Generate a contract. With a module (modular orchestrator): TypeScript types of a microservice API in modules/<module>/contracts/; without: a JSON service contract (src/.app/contracts/, 0.1: src/contracts/)

Generate a contract. With a module (modular orchestrator): TypeScript types of a microservice API in modules/<module>/contracts/; without: a JSON service contract (src/.app/contracts/, 0.1: src/contracts/).

## Usage

```text
pso make:contract <module> <name> [--service <orchestrator>] [--dry-run] [--json] | pso make:contract <name> [--route /path] [--method GET] [--service <name>] [--dry-run] [--force] [--json]
```

## Flags

`--service`, `--dry-run`, `--json`, `--route`, `--method`, `--force`.

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
