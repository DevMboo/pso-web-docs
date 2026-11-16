# pso release:endpoints

> The routes published WITH the active release (method, effective URL, controller, service, guard, required scopes), from release metadata; a release without metadata says so. Declared, deployed and verified are different states: nothing here is 'verified' until a request proves it

The routes published WITH the active release (method, effective URL, controller, service, guard, required scopes), from release metadata; a release without metadata says so. Declared, deployed and verified are different states: nothing here is 'verified' until a request proves it.

## Usage

```text
pso release:endpoints <application> [--json]
```

## Flags

`--json`.

## Examples

```bash
pso release:endpoints shop-gateway
pso release:endpoints shop-gateway --json
```

## What it changes

- Lists the routes published with the ACTIVE release (pso-release.json written at build time by `pso routes:list --out`).

## Failure cases

- `no endpoints`: the release carries no route metadata; nothing is invented. Spring controllers behind the orchestrator are not enumerated.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).
