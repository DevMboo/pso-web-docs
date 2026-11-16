# pso release:deploy

> Deploy: with --plan, exactly that approved plan; without it, a fresh plan is made, shown and executed. The deployment runs on the server and continues if this command or your connection ends

Deploy: with --plan, exactly that approved plan; without it, a fresh plan is made, shown and executed. The deployment runs on the server and continues if this command or your connection ends.

## Usage

```text
pso release:deploy <environment> [--plan <hash>] [--branch <branch>] [--tag <tag>] [--commit <sha>] [--signed <file>] [--approve] [--no-follow] --yes [--json]
```

## Flags

`--plan`, `--branch`, `--tag`, `--commit`, `--signed`, `--approve`, `--no-follow`, `--yes`, `--json`.

## Examples

```bash
pso release:deploy production --branch main --yes
pso release:deploy production --plan 3f9c1a... --yes
pso release:deploy production --tag v1.4.0 --approve --no-follow --yes
```

## What it changes

- Runs the deployment on the server (detached from this command): validate, prepare an isolated release, build, start the candidate, verify, switch traffic, drain the previous release.

## Failure cases

- Exit code 3 without `--yes`.
- `PLAN_STALE`: the branch moved, the active release changed or the policy changed after the plan; the latest commit is never deployed implicitly.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [cloud/deploy-a-release](cloud/deploy-a-release.md)
- [cloud/rollback-and-recovery](cloud/rollback-and-recovery.md)
