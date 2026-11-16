# pso fei:unregister

> Remove a MEI from FEI.pso.ts (the MEI project itself is not deleted)

Remove a MEI from FEI.pso.ts (the MEI project itself is not deleted).

## Usage

```text
pso fei:unregister <mei> [--dry-run]
```

## Flags

`--dry-run`.

## Examples

```bash
pso fei:unregister customers
```

## What it changes

- Removes the entry from `FEI.pso.ts`. The project directory and its workspace registration stay.

## Failure cases

- `is not registered`: nothing to remove.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [frontend/declarations](frontend/declarations.md)
