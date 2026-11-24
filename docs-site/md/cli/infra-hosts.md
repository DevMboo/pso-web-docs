# pso infra:hosts

> Show the hosts entries a development environment needs; with --apply (or --remove) and --yes write (or remove) PSO's marked block in the hosts file, nothing else (the system file needs administrator rights). Browsers resolve *.localhost without it

Show the hosts entries a development environment needs; with --apply (or --remove) and --yes write (or remove) PSO's marked block in the hosts file, nothing else (the system file needs administrator rights). Browsers resolve *.localhost without it.

## Usage

```text
pso infra:hosts [--env <name>] [--apply | --remove] [--file <hosts-file>] [--yes] [--json]
```

## Flags

`--env`, `--apply`, `--remove`, `--file`, `--yes`, `--json`.

## Examples

```bash
pso infra:hosts
pso infra:hosts --apply --yes
pso infra:hosts --remove --yes
pso infra:hosts --file ./hosts-copy --apply --yes
```

## What it changes

- Without `--apply`/`--remove` it only prints the block. With them and `--yes` it writes or removes PSO's marked block; no other line of the file changes and the previous file is kept as `<file>.pso-backup`.

## Failure cases

- Writing the system hosts file needs administrator rights: PSO never elevates itself.
- Staging and production need real DNS records: refused.
- Exit code 3 without `--yes`.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [infrastructure/hosts-and-environments](infrastructure/hosts-and-environments.md)
