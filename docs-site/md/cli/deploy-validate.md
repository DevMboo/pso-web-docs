# pso deploy:validate

> Statically check the deployment descriptors (security posture, probes, limits, inline secrets, unfilled placeholders); no cloud is contacted

Statically check the deployment descriptors (security posture, probes, limits, inline secrets, unfilled placeholders); no cloud is contacted.

## Usage

```text
pso deploy:validate [--target <kubernetes|ecs|container-apps|cloud-run>] [--strict] [--json]
```

## Flags

`--target`, `--strict`, `--json`.

## Examples

```bash
pso deploy:validate
pso deploy:validate --target kubernetes --strict
pso deploy:validate --json
```

## Failure cases

- Static checks only: no cloud account or cluster is contacted. Policy violations (root user, writable root file system, missing probes or limits, inline secrets, `latest` tags) exit 1; unfilled CHANGE_ME values are warnings, and failures with --strict.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).
