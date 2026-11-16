# pso release:approve

> Approve a proposed plan (the server policy decides whether the proposer may approve their own plan). The approval is bound to the plan hash, the commit and the policy

Approve a proposed plan (the server policy decides whether the proposer may approve their own plan). The approval is bound to the plan hash, the commit and the policy.

## Usage

```text
pso release:approve <environment> --plan <hash> [--json]
```

## Flags

`--plan`, `--json`.

## Examples

```bash
pso release:approve production --plan 3f9c1a...
```

## Failure cases

- Under a policy without self-approval the proposer cannot approve their own plan.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [cloud/pipelines-and-policy](cloud/pipelines-and-policy.md)
