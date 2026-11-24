# pso infra:apply

> Provision the environment idempotently: create what is missing and PSO may generate (development only), update configuration whose declarations changed, keep everything valid, never touch operator-supplied resources or hand edits

Provision the environment idempotently: create what is missing and PSO may generate (development only), update configuration whose declarations changed, keep everything valid, never touch operator-supplied resources or hand edits.

## Usage

```text
pso infra:apply [--env <name>] [--only <id,id>] [--dry-run] [--yes] [--json]
```

## Flags

`--env`, `--only`, `--dry-run`, `--yes`, `--json`.

## Examples

```bash
pso infra:apply
pso infra:apply --env staging --yes
pso infra:apply --only cert:edge,nginx:edge
pso infra:apply --dry-run --json
```

## What it changes

- Creates what is missing and authorized (development material only in development environments), updates configuration whose declarations changed, restricts exposed key files. Valid resources, operator-supplied resources and hand edits are never touched.
- Prints which running containers need a restart.

## Verify the result

- A second run prints `nothing to do`.
- `pso infra:status` reports `healthy`.

## Failure cases

- Exit code 3 for a staging or production environment without `--yes`.
- Exit code 1 when an operator-supplied resource is missing or invalid: PSO never generates it there.
- `another PSO process ... is changing this workspace's infrastructure`: wait; a stale lock is taken over automatically.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [infrastructure/resource-lifecycle](infrastructure/resource-lifecycle.md)
- [infrastructure/security-and-certificates](infrastructure/security-and-certificates.md)
