# pso environment:provision

> Apply the unprivileged provisioning steps (agent directories, default policy). Idempotent. Privileged steps are only listed: PSO never installs packages, edits nginx.conf, changes firewall rules or replaces existing configuration

Apply the unprivileged provisioning steps (agent directories, default policy). Idempotent. Privileged steps are only listed: PSO never installs packages, edits nginx.conf, changes firewall rules or replaces existing configuration.

## Usage

```text
pso environment:provision <environment> --yes [--json]
```

## Flags

`--yes`, `--json`.

## Examples

```bash
pso environment:provision production --yes
```

## Failure cases

- Exit code 3 without `--yes`. Privileged steps are only listed: PSO never installs packages, edits nginx.conf or changes firewall rules.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [cloud/nginx-and-existing-infrastructure](cloud/nginx-and-existing-infrastructure.md)
