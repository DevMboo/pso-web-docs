# pso make:decorator

> Generate a reusable method decorator in src/.app/decorators/ (modular orchestrator): application metadata, never routing or security

Generate a reusable method decorator in src/.app/decorators/ (modular orchestrator): application metadata, never routing or security.

## Usage

```text
pso make:decorator <name> [--service <name>] [--dry-run] [--force] [--json]
```

## Flags

`--service`, `--dry-run`, `--force`, `--json`.

## Examples

```bash
pso make:decorator audited
```

## What it changes

- Writes `src/.app/decorators/<name>.decorator.ts`, imported where it is used.

## Failure cases

- `needs the modular structure`: see `pso structure:inspect`.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [architecture/typescript-orchestrator](architecture/typescript-orchestrator.md)
