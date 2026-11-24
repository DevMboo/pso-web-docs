# pso infra:repair

> Resolve drift and damage: regenerate managed files that were edited by hand (the edited copy is kept in .infra/backups/), complete interrupted provisioning, restrict exposed key files. Needs --yes when something would be overwritten

Resolve drift and damage: regenerate managed files that were edited by hand (the edited copy is kept in .infra/backups/), complete interrupted provisioning, restrict exposed key files. Needs --yes when something would be overwritten.

## Usage

```text
pso infra:repair [--env <name>] [--yes] [--json]
```

## Flags

`--env`, `--yes`, `--json`.

## Examples

```bash
pso infra:repair
pso infra:repair --yes
pso infra:repair --env staging --yes
```

## What it changes

- Regenerates managed files that were edited by hand (the edited copy is kept in `.infra/backups/<run>/`), completes interrupted provisioning and restricts exposed key files.

## Failure cases

- Exit code 3 without `--yes` when a file would be overwritten: the command lists them first.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [infrastructure/resource-lifecycle](infrastructure/resource-lifecycle.md)
