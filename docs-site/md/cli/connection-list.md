# pso connection:list

> List registered connections (references only: no secret is ever shown)

List registered connections (references only: no secret is ever shown).

## Usage

```text
pso connection:list [--json]
```

## Flags

`--json`.

## Examples

```bash
pso connection:list
pso connection:list --json
```

## Failure cases

- Only references are listed: no secret is ever shown.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [cloud/connect-a-server](cloud/connect-a-server.md)
