# pso infra:rotate

> Replace a valid certificate, authority, signing key or secret on purpose (rotation). Replacing the authority re-issues every certificate it signed. Development environments only: production material is rotated by its owner

Replace a valid certificate, authority, signing key or secret on purpose (rotation). Replacing the authority re-issues every certificate it signed. Development environments only: production material is rotated by its owner.

## Usage

```text
pso infra:rotate <resource> [--env <name>] --yes [--json]
```

## Flags

`--env`, `--yes`, `--json`.

## Examples

```bash
pso infra:rotate cert:edge --yes
pso infra:rotate ca --yes
pso infra:rotate secret:db-catalog --env local --yes
```

## What it changes

- Replaces a valid PSO-owned certificate, authority, signing key or secret. Rotating the authority re-issues every certificate it signed. Running containers keep the old material until they restart.

## Failure cases

- Exit code 3 without `--yes`.
- Configuration (`nginx:*`, `docker:*`) is not key material and is refused.
- Operator-supplied material is rotated at its source, not by PSO.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [infrastructure/security-and-certificates](infrastructure/security-and-certificates.md)
