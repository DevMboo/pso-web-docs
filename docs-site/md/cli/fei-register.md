# pso fei:register

> Register a MEI with the FEI: adds it to FEI.pso.ts (mount path, entry scopes, loading) and validates the relationship

Register a MEI with the FEI: adds it to FEI.pso.ts (mount path, entry scopes, loading) and validates the relationship.

## Usage

```text
pso fei:register <mei> [--fei <name>] [--path /prefix] [--scopes a:read,b:write] [--public] [--title <text>] [--icon <name>] [--loading lazy|eager] [--dry-run]
```

## Flags

`--fei`, `--path`, `--scopes`, `--public`, `--title`, `--icon`, `--loading`, `--dry-run`.

## Examples

```bash
pso fei:register customers
pso fei:register catalog --path /products --scopes catalog:read --title Catalog --icon catalog
pso fei:register orders --dry-run
```

## What it changes

- Adds an entry to `microfrontends` in `FEI.pso.ts` (mount path, entry scopes, loading mode) and prints the findings that mention the new MEI.
- Only the one file changes; the MEI project is not touched.

## Failure cases

- The path overlaps another microfrontend's mount: names both and refuses.
- `is not a MEI of this workspace`: create it first with `pso create mei <domain>`.
- A `FEI.pso.ts` edited so that `microfrontends` is no longer a literal array cannot be edited by PSO: add the entry by hand.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [frontend/declarations](frontend/declarations.md)
