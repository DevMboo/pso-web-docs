# pso infra:status

> One-screen health of an environment's infrastructure: resources by status, drift, certificates close to expiry, the last provisioning and whether one was interrupted

One-screen health of an environment's infrastructure: resources by status, drift, certificates close to expiry, the last provisioning and whether one was interrupted.

## Usage

```text
pso infra:status [--env <name>] [--json]
```

## Flags

`--env`, `--json`.

## Examples

```bash
pso infra:status
pso infra:status --env staging --json
```

## Illustrative output

This sample shows the shape of the result. Names, ports, digests and timings may differ in your workspace.

```text evidence="illustrative"
local: healthy
  27 resources: 27 ok
  last apply: 2026-11-24T09:25:02.928Z by pso deploy (1 change(s))
  nearest certificate expiry: cert:edge in 396 day(s)
```

## Failure cases

- Exit code 1 when the environment needs attention (pending changes, drift, missing supplied resources, an interrupted run); the last line names the command to run.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).
