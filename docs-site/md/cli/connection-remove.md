# pso connection:remove

> Delete a connection from this machine (credentials and pinned host key included); the server and its applications are not touched

Delete a connection from this machine (credentials and pinned host key included); the server and its applications are not touched.

## Usage

```text
pso connection:remove <name> --yes [--json]
```

## Flags

`--yes`, `--json`.

## Examples

```bash
pso connection:remove prod --yes
```

## Failure cases

- Exit code 3 without `--yes`. Nothing on the server is changed.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [cloud/connect-a-server](cloud/connect-a-server.md)
