# pso make:repository

> Generate a repository interface (Spring: persistence/). It does not enable a database: see `pso database:enable`

Generate a repository interface (Spring: persistence/). It does not enable a database: see `pso database:enable`.

## Usage

```text
pso make:repository <name> [--service <name>] [--dry-run] [--force] [--json]
```

## Flags

`--service`, `--dry-run`, `--force`, `--json`.

## Examples

```bash
pso make:repository product --service catalog --dry-run
pso make:repository product --service catalog
```

## Failure cases

- A repository stub does not enable a database; select a provider with `pso database:enable` when persistence is required.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [microservices/persistence](microservices/persistence.md)
