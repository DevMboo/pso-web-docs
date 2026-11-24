# pso host:inspect

> Inspect a registered Linux host over SSH (read-only): operating system and architecture, Docker and Compose available to the deploy user, free disk under the deployment root, listening ports, existing PSO-managed deployments and registry reachability

Inspect a registered Linux host over SSH (read-only): operating system and architecture, Docker and Compose available to the deploy user, free disk under the deployment root, listening ports, existing PSO-managed deployments and registry reachability.

## Usage

```text
pso host:inspect <connection> [--root /srv/pso] [--json]
```

## Flags

`--root`, `--json`.

## Examples

```bash
pso host:inspect staging-host
pso host:inspect staging-host --json
```

## What it changes

- Read-only over SSH (pinned host key): operating system and architecture, Docker and Compose usable by the deploy user, free disk, listening ports, existing PSO deployments, registry reachability. Nothing is installed or changed.

## Illustrative output

This sample shows the shape of the result. Names, ports, digests and timings may differ in your workspace.

```text evidence="illustrative"
staging-host: pso@127.0.0.1:2222 (pso)
  system     Alpine Linux v3.21, x86_64, kernel 6.18.33.2-microsoft-standard-WSL2
  docker     27.5.1   compose 2.33.0
  root       /srv/pso: writable, 922051 MB free
  listening  22, 9443
  managed    pso-reference-staging (active 20261124113940-6fe8af7f)
  registry   Docker Hub resolvable
```

## Failure cases

- `cannot inspect <name>: ... Host key verification failed`: the host presents another key than the pinned one (OpenSSH refuses it); nothing was sent. Find out why before re-registering.
- An unregistered name: the hint shows the `pso connection:add` to run.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [infrastructure/remote-hosts](infrastructure/remote-hosts.md)
