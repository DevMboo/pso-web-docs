# pso deploy:status

> Show the active release of an environment, its recorded releases and history, and the state and health of its containers (on the remote host for an SSH target, on this machine for the docker target)

Show the active release of an environment, its recorded releases and history, and the state and health of its containers (on the remote host for an SSH target, on this machine for the docker target).

## Usage

```text
pso deploy:status --env <name> [--json]
```

## Flags

`--env`, `--json`.

## Examples

```bash
pso deploy:status --env staging
pso deploy:status --env staging --json
```

## What it changes

- Read-only: the active release on the host, the releases kept there, the containers of the active release with their health, the deployment history and whether a deployment lock is held.

## Illustrative output

This sample shows the shape of the result. Names, ports, digests and timings may differ in your workspace.

```text evidence="illustrative"
staging on staging-host: active 20261124113940-6fe8af7f
  releases: 20261124112815-6fe8af7f, 20261124113053-6fe8af7f, 20261124113321-6fe8af7f, 20261124113940-6fe8af7f
  accounts         running   healthy
  catalog          running   healthy
  ...
  idp              running   healthy
```

## Failure cases

- `environment "<name>" is not deployed over SSH`: its `deploy.target` is not `ssh` (local environments: `pso docker:ps`).

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [infrastructure/remote-hosts](infrastructure/remote-hosts.md)
