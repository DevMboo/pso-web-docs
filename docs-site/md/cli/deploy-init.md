# pso deploy:init

> Generate hardened deployment descriptors for Kubernetes, AWS ECS (Fargate), Azure Container Apps or Google Cloud Run (nothing is ever applied to a cloud)

Generate hardened deployment descriptors for Kubernetes, AWS ECS (Fargate), Azure Container Apps or Google Cloud Run (nothing is ever applied to a cloud).

## Usage

```text
pso deploy:init --target <kubernetes|ecs|container-apps|cloud-run> [--registry <registry/path>] [--tag <version>] [--dry-run] [--force] [--json]
```

## Flags

`--target`, `--registry`, `--tag`, `--dry-run`, `--force`, `--json`.

## Examples

```bash
pso deploy:init --target kubernetes --registry registry.example.com/pso --tag 1.2.3 --dry-run
pso deploy:init --target ecs --registry registry.example.com/pso --tag 1.2.3
pso deploy:init --target cloud-run --registry registry.example.com/pso --tag 1.2.3
```

## Failure cases

- Files are written under deploy/<target>/ and nothing is applied to any cloud. A mutable `latest` tag is refused, and an existing file that differs is a conflict unless you pass --force.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).
