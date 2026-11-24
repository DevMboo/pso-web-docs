# pso frontend:publish

> Publish a built FEI or MEI release to a site root: immutable versioned files, compatibility and integrity checks, then one atomic activation (nothing already published is overwritten or deleted)

Publish a built FEI or MEI release to a site root: immutable versioned files, compatibility and integrity checks, then one atomic activation (nothing already published is overwritten or deleted).

## Usage

```text
pso frontend:publish <project> --site <dir> | --env <name> [--version 1.2.0] [--base-path /commerce/] [--stage] [--json] | --all --env <name> [--json]
```

## Flags

`--site`, `--env`, `--version`, `--base-path`, `--stage`, `--json`, `--all`.

## Examples

```bash
pso frontend:publish fei-commerce --site ./site --base-path /commerce/
pso frontend:publish customers --site ./site
pso frontend:publish customers --site ./site --stage
```

## What it changes

- Copies the build into an immutable versioned directory of the site root and activates it with one atomic pointer write; nothing published earlier is changed or deleted. `--stage` copies without activating.

## Illustrative output

This sample shows the shape of the result. Names, ports, digests and timings may differ in your workspace.

```text evidence="illustrative"
mei-customers 0.2.0 is active (previous: 0.1.0; signed by release-1)
```

## Failure cases

- `no FEI release is active`: publish the shell first.
- `already published with different content`: published versions are immutable; increase the version.
- A release built for another React major or shared-context major, or with a signature the site does not trust, is refused and not copied.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [frontend/releases](frontend/releases.md)
