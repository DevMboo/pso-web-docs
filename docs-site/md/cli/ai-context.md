# pso ai:context

> Generate AI-assistant context from the installed framework and the current project (never includes secrets)

Generate AI-assistant context from the installed framework and the current project (never includes secrets).

## Usage

```text
pso ai:context [--list] [--scope a,b] [--format markdown|json|agents|claude|copilot] [--output <file>] [--no-project] [--dry-run] [--force] [--json]
```

## Flags

`--list`, `--scope`, `--format`, `--output`, `--no-project`, `--dry-run`, `--force`, `--json`.

## Examples

```bash
pso ai:context --list
pso ai:context --scope security,module-routes
pso ai:context --format markdown --output ./PSO_CONTEXT.md
pso ai:context --format agents --output ./AGENTS.md --dry-run
```

## Failure cases

- `unknown scope`: use `pso ai:context --list`.
- An existing file is never overwritten without `--force`; managed blocks between `pso:ai-context` markers are updated in place.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [ai-context/overview](ai-context/overview.md)
- [ai-context/assistant-files](ai-context/assistant-files.md)
