# pso ai:init

> Inspect the workspace and create AGENTS.md, CLAUDE.md and Copilot instructions with project-specific PSO guidance

Inspect the workspace and create AGENTS.md, CLAUDE.md and Copilot instructions with project-specific PSO guidance.

## Usage

```text
pso ai:init [--target all|agents,claude,copilot] [--scope a,b] [--no-project] [--dry-run] [--force] [--json]
```

## Flags

`--target`, `--scope`, `--no-project`, `--dry-run`, `--force`, `--json`.

## Examples

```bash
pso ai:init --dry-run
pso ai:init
pso ai:init --target agents,claude
```

## Failure cases

- Existing AGENTS.md / CLAUDE.md / copilot-instructions.md without a PSO block are skipped unless `--force`, which appends a managed block and never deletes your text.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [ai-context/assistant-files](ai-context/assistant-files.md)
