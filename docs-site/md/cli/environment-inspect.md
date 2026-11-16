# pso environment:inspect

> Inspect the remote server (OS, CPU, memory, disk, ports, Java, Node, git, nginx, existing PSO apps and vhosts); read-only

Inspect the remote server (OS, CPU, memory, disk, ports, Java, Node, git, nginx, existing PSO apps and vhosts); read-only.

## Usage

```text
pso environment:inspect <environment> [--json]
```

## Flags

`--json`.

## Examples

```bash
pso environment:inspect production
pso environment:inspect production --json
```

## Failure cases

- Exit code 1 when the server has blockers (for example nginx's configuration does not pass `nginx -t`).

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [cloud/nginx-and-existing-infrastructure](cloud/nginx-and-existing-infrastructure.md)
