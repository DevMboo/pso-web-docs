# pso structure:inspect

> Show the structural convention of each project (modular / layered / 0.1), mixed or contradictory structures, and for a 0.1 project the migration plan: every file to move, the imports, route declarations, module registrations and Java package declarations it affects. Read-only: nothing is moved

Show the structural convention of each project (modular / layered / 0.1), mixed or contradictory structures, and for a 0.1 project the migration plan: every file to move, the imports, route declarations, module registrations and Java package declarations it affects. Read-only: nothing is moved.

## Usage

```text
pso structure:inspect [--service <name>] [--json]
```

## Flags

`--service`, `--json`.

## Examples

```bash
pso structure:inspect
pso structure:inspect --service gateway --json
```

## What it changes

- Read-only: reports each project's structure and, for a 0.1 project, the migration plan (moves, import and package updates, decisions). Nothing is moved.

## Failure cases

- `PSO_STRUCTURE_MIXED`: the project has files of both structures; keep one before migrating.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [architecture/structure-migration](architecture/structure-migration.md)
