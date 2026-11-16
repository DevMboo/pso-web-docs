# pso frontend:install

> Vendor the framework packages into the frontend projects and run `npm install` in each (or only in <project>)

Vendor the framework packages into the frontend projects and run `npm install` in each (or only in <project>).

## Usage

```text
pso frontend:install [<project>] [--dry-run]
```

## Flags

`--dry-run`.

## Examples

```bash
pso frontend:install
pso frontend:install customers
pso frontend:install --dry-run
```

## What it changes

- Packs the framework packages this CLI ships into `vendor/pso` of each frontend project, rewrites its dependencies to them and runs `npm install`.

## Failure cases

- After a framework update npm may keep the old copy because the tarball has the same name and version: remove `node_modules/@pso` and `package-lock.json` first.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [frontend/getting-started](frontend/getting-started.md)
