# pso frontend:dev

> Start the development servers: the FEI and every registered MEI (ports from their declarations), or one MEI alone with the development host adapter (--standalone)

Start the development servers: the FEI and every registered MEI (ports from their declarations), or one MEI alone with the development host adapter (--standalone).

## Usage

```text
pso frontend:dev [--standalone <mei>] [--only <project>] [--no-install-check]
```

## Flags

`--standalone`, `--only`, `--no-install-check`.

## Examples

```bash
pso frontend:dev
pso frontend:dev --standalone customers
pso frontend:dev --only fei-shop
```

## What it changes

- Starts the declared development servers: the shell, every registered MEI, and a proxy from `/api` to the orchestrator. `--standalone <mei>` starts one MEI with the development host adapter (a simulated shell that exists only in that entry).

## Failure cases

- `Port N (project) is already in use`: nothing is started.
- `has no installed dependencies`: run `pso frontend:install`.
- Declaration errors stop the start; fix them with `pso frontend:validate`.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [frontend/getting-started](frontend/getting-started.md)
