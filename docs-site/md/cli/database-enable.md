# pso database:enable

> Opt a Spring service into a database: adds the persistence starter and sets pso.database.provider (H2 only for the local profile)

Opt a Spring service into a database: adds the persistence starter and sets pso.database.provider (H2 only for the local profile).

## Usage

```text
pso database:enable <h2|postgresql> [--service <name>] [--dry-run] [--json]
```

## Flags

`--service`, `--dry-run`, `--json`.

## Examples

```bash
pso database:enable postgresql --service orders --dry-run
pso database:enable postgresql --service orders
pso database:enable h2 --service orders
```

## Failure cases

- Only Spring services are supported. H2 is confined to the local profile and is refused for staging or production.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [spring/database-and-h2](spring/database-and-h2.md)
