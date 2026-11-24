# pso host:check

> Check that the host an environment deploys to (deploy.target "ssh") meets its prerequisites: Docker and Compose, a writable root with free space, the edge port, and every operator-supplied certificate (verified: authority, names, expiry; keys present) and secret file (present; never read). Exit 1 when something blocks a deployment

Check that the host an environment deploys to (deploy.target "ssh") meets its prerequisites: Docker and Compose, a writable root with free space, the edge port, and every operator-supplied certificate (verified: authority, names, expiry; keys present) and secret file (present; never read). Exit 1 when something blocks a deployment.

## Usage

```text
pso host:check --env <name> [--json]
```

## Flags

`--env`, `--json`.

## Examples

```bash
pso host:check --env staging
pso host:check --env production --json
```

## What it changes

- The prerequisites of one environment on its host: Docker and Compose, writable deployment root, disk, the edge port, the operator's certificates (issued by the authority, valid, with the required names; keys only tested for presence and readability) and the secret files (presence only, never read).

## Illustrative output

This sample shows the shape of the result. Names, ports, digests and timings may differ in your workspace.

```text evidence="illustrative"
INFO    docker    Docker 27.5.1
INFO    tls       6 certificate(s) verified on the host (authority, names, expiry; keys present)
INFO    secrets   2 secret file(s) present (contents not read)
OK: staging-host can run "staging"
```

## Failure cases

- Exit 1 with the list of blocking problems and what the administrator must do. PSO never installs packages, changes the firewall or creates users.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [infrastructure/remote-hosts](infrastructure/remote-hosts.md)
