# pso install:react

> Install an official PSO integration into an existing react application: detects the project and its package manager, installs the packages, writes small application-owned files, never overwrites yours, then type-checks the result

Install an official PSO integration into an existing react application: detects the project and its package manager, installs the packages, writes small application-owned files, never overwrites yours, then type-checks the result.

## Usage

```text
pso install:react <channels> [--dir <folder>] [--contracts <orchestrator>] [--dry-run] [--skip-install] [--json]
```

## Flags

| Flag | What it does |
|---|---|
| `--dir` | Folder for the generated files (default src/integrations/pso). |
| `--contracts` | Orchestrator folder to generate the typed contracts from (found automatically in a PSO workspace). |
| `--dry-run` | Show packages and files without changing anything. |
| `--skip-install` | Write the files only; add the packages yourself. |
| `--json` | Print the report as JSON. |

## Examples

```bash
pso install:react channels --dry-run
pso install:react channels
pso install:react channels --contracts ../gw
pso install:react channels --dir src/lib/pso --skip-install
```

## What it changes

- Installs `@pso/channels-client` and `@pso/react-channels` with the project's own package manager (npm, pnpm or Yarn, from the packageManager field or the lockfile) and writes `client.ts`, `channels.ts` and `types.ts` under `src/integrations/pso/`.
- Existing files are never overwritten and App.tsx is never edited; the provider snippet is printed for you to add.

## Verify the result

- The generated files are type-checked with the project's own TypeScript options; the result is reported before the command ends.
- Running it again reports `complete` and changes nothing.

## Failure cases

- The project must be an existing React 18+ application with TypeScript and a src folder. A failed package installation restores package.json and the lockfile and writes nothing. A file that exists and was not written by PSO stops the command.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [orchestrator/channels](orchestrator/channels.md)
