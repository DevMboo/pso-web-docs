# pso config:list

> List configuration sources found per scope and the known configuration keys

List configuration sources found per scope and the known configuration keys.

## Usage

```text
pso config:list [--service name] [--env development|test|staging|production] [--json]
```

## Flags

`--service`, `--env`, `--json`.

## Examples

```bash
pso config:list
pso config:list --service gateway --env production
```

## Failure cases

- This lists sources and known keys; use `config:show` for effective redacted values and `config:validate` for errors.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [concepts/configuration](concepts/configuration.md)
