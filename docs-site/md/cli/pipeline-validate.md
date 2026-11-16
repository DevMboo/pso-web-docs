# pso pipeline:validate

> Statically validate deployment and pipeline declarations (no network, no code executed): structure, secrets in declarations, strategy feasibility, stage order and dependencies

Statically validate deployment and pipeline declarations (no network, no code executed): structure, secrets in declarations, strategy feasibility, stage order and dependencies.

## Usage

```text
pso pipeline:validate [<environment>] [--json]
```

## Flags

`--json`.

## Examples

```bash
pso pipeline:validate
pso pipeline:validate production --json
```

## What it changes

- Statically validates deployment and pipeline declarations without executing project code or contacting a server.

## Failure cases

- `PSO-DEC-STATIC`: a declaration computed a value; declarations may hold literal data only.
- `PSO-DEC-SECRET`: a credential was written into a declaration.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [cloud/deployment-declarations](cloud/deployment-declarations.md)
