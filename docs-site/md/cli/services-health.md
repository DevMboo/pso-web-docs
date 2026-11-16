# pso services:health

> Probe every declared service live (exit 1 if any is DOWN)

Probe every declared service live (exit 1 if any is DOWN).

## Usage

```text
pso services:health [--json]
```

## Flags

`--json`.

## Examples

```bash
pso services:health
pso services:health --json
```

## What it changes

- Probes each declared health endpoint; it changes no configuration.

## Illustrative output

This sample shows the shape of the result. Names, ports, digests and timings may differ in your workspace.

```text evidence="illustrative"
UP    gateway    http://127.0.0.1:3000/health/ready  4ms
DOWN  catalog    http://127.0.0.1:8081/actuator/health
```

## Verify the result

- A DOWN row names the URL to inspect in that service's log.
- All UP rows establish reachability, not correctness of a protected business endpoint.

## Failure cases

- Exit code 1 when any declared service is DOWN. It probes live; it never trusts the registry file.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).
