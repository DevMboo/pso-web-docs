# pso release:plan

> Ask the server for a deployment plan: resolves the branch or tag to an immutable commit, checks resources, strategy, nginx, policy and pipeline. Changes nothing but the stored proposal

Ask the server for a deployment plan: resolves the branch or tag to an immutable commit, checks resources, strategy, nginx, policy and pipeline. Changes nothing but the stored proposal.

## Usage

```text
pso release:plan <environment> [--branch <branch>] [--tag <tag>] [--commit <sha>] [--signed <file>] [--json]
```

## Flags

`--branch`, `--tag`, `--commit`, `--signed`, `--json`.

## Examples

```bash
pso release:plan production
pso release:plan production --branch main
pso release:plan production --tag v1.4.0 --json
```

## What it changes

- Asks the server to inspect itself and pin the branch or tag to a commit; prints the plan and its hash. Stores only the proposal.

## Failure cases

- Exit code 1 when the plan has blockers; they are listed with the reason (never silently downgraded to a weaker strategy).

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [cloud/deploy-a-release](cloud/deploy-a-release.md)
