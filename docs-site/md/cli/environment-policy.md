# pso environment:policy

> Show the server-side deployment policy (mode, approvals, allowed strategies, retention...); --set changes it (superadministrator, audited). The server policy overrides anything a project declares

Show the server-side deployment policy (mode, approvals, allowed strategies, retention...); --set changes it (superadministrator, audited). The server policy overrides anything a project declares.

## Usage

```text
pso environment:policy <environment> [--set key=value] [--compat] [--migrate <id>] [--history] [--revert <entry>] [--yes] [--json]
```

## Flags

`--set`, `--compat`, `--migrate`, `--history`, `--revert`, `--yes`, `--json`.

## Examples

```bash
pso environment:policy production
pso environment:policy production --set approvalsRequired=1 --set selfApproval=false --yes
pso environment:policy production --compat
pso environment:policy production --migrate allow-static-frontends
pso environment:policy production --history
```

## Failure cases

- Changing policy needs the superadministrator role and `--yes`; the change is audited on the server. The server policy overrides anything a project declares.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [cloud/pipelines-and-policy](cloud/pipelines-and-policy.md)
