# pso infra:inspect

> Show the resolved topology of an environment (hosts, origins, OIDC redirect URIs, addresses as containers and as processes see them) and its resources with owner, files and facts (fingerprints, expiry; never key material or secret values)

Show the resolved topology of an environment (hosts, origins, OIDC redirect URIs, addresses as containers and as processes see them) and its resources with owner, files and facts (fingerprints, expiry; never key material or secret values).

## Usage

```text
pso infra:inspect [<resource>] [--env <name>] [--json]
```

## Flags

`--env`, `--json`.

## Examples

```bash
pso infra:inspect
pso infra:inspect cert:edge
pso infra:inspect --env staging --json
```

## What it changes

- Prints the resolved topology (hosts, OIDC redirect URIs, addresses as containers and as processes see them) and every resource with owner, files and facts. Key material and secret values are never printed.

## Failure cases

- `Unknown resource`: the message lists the valid ids.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [infrastructure/hosts-and-environments](infrastructure/hosts-and-environments.md)
