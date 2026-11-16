# pso agent:install

> Install the runtime agent over SSH, provision it (server identity, policy, a single-use administrator enrollment) and enroll this machine as its first superadministrator. Remote management stays disabled unless you choose a mode

Install the runtime agent over SSH, provision it (server identity, policy, a single-use administrator enrollment) and enroll this machine as its first superadministrator. Remote management stays disabled unless you choose a mode.

## Usage

```text
pso agent:install <connection> --mode development|protected --environment <name> --yes [--json]
```

## Flags

`--mode`, `--environment`, `--yes`, `--json`.

## Examples

```bash
pso agent:install prod --mode protected --environment production --yes
pso agent:install staging --mode development --environment staging --yes
```

## What it changes

- Uploads the bundled agent over SSH, verifies its checksum, provisions the server identity and policy, and enrolls this machine as the first superadministrator with a single-use token.

## Failure cases

- Exit code 3 without `--yes`; `--mode` is mandatory because remote management is disabled by default.
- `already provisioned`: ask an administrator for an enrollment token instead (`pso environment:access --invite`).

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [cloud/agent-and-enrollment](cloud/agent-and-enrollment.md)
