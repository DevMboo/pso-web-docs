# pso infra:plan

> Show what the infrastructure of an environment needs and what applying would do (create, replace, update, supply), without changing anything

Show what the infrastructure of an environment needs and what applying would do (create, replace, update, supply), without changing anything.

## Usage

```text
pso infra:plan [--env <name>] [--all] [--json]
```

## Flags

`--env`, `--all`, `--json`.

## Examples

```bash
pso infra:plan
pso infra:plan --env staging --all
pso infra:plan --json
```

## What it changes

- Inspects every resource of the environment (files, chains, keys, names, expiry, permissions, drift) and prints what applying would do. Nothing is changed.

## Illustrative output

This sample shows the shape of the result. Names, ports, digests and timings may differ in your workspace.

```text evidence="illustrative"
environment local (development)   runtime docker   tls pso-dev-ca   secrets local
  tls
    MISSING            cert:edge                      -> create  (not issued yet)
27 change(s), 0 drifted, 0 to supply, 0 unchanged
```

## Failure cases

- `pso.infra.json is invalid`: each problem names the member and the fix (for example PSO_INFRA_TLS_PRODUCTION).
- `unknown environment`: use a name declared under `environments`.
- A host collision between shells, the API and the identity provider is refused (PSO_INFRA_HOST_CONFLICT).

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [infrastructure/resource-lifecycle](infrastructure/resource-lifecycle.md)
