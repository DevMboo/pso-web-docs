# pso init

> Create pso.workspace.json in the current directory from the Node/Spring projects found in it

Create pso.workspace.json in the current directory from the Node/Spring projects found in it.

## Usage

```text
pso init [--dry-run] [--force] [--json]
```

## Flags

`--dry-run`, `--force`, `--json`.

## Examples

```bash
cd existing-projects
pso init --dry-run
pso init
```

## Illustrative output

This sample shows the shape of the result. Names, ports, digests and timings may differ in your workspace.

```text evidence="illustrative"
pso.workspace.json: 2 project(s) found
  users (spring-boot, service) :8081
  edge (node, orchestrator) :3000
```

## Failure cases

- Refuses to overwrite an existing pso.workspace.json without `--force`.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [concepts/path-structure-working](concepts/path-structure-working.md)
