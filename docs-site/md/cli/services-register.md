# pso services:register

> Declare an existing project as a workspace service (validated before saving)

Declare an existing project as a workspace service (validated before saving).

## Usage

```text
pso services:register <name> --port N --runtime node|spring-boot [--path dir] [--depends-on a,b] [--health /path] [--dry-run] [--json]
```

## Flags

`--port`, `--runtime`, `--path`, `--depends-on`, `--health`, `--dry-run`, `--json`.

## Examples

```bash
pso services:register catalog --port 8081 --runtime spring-boot --path catalog --dry-run
pso services:register catalog --port 8081 --runtime spring-boot --path catalog
```

## Failure cases

- The existing project is validated before its declaration is saved; a port collision or missing project prevents registration.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [microservices/service-registration](microservices/service-registration.md)
