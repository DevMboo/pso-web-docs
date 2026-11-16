# pso doctor

> Check the local toolchain

Check the local toolchain.

## Usage

```text
pso doctor [--json]
```

## Flags

`--json`.

## Examples

```bash
pso doctor
pso doctor --json
```

## Failure cases

- A failed toolchain check names the missing local tool; it does not install it for you.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [getting-started/requirements](getting-started/requirements.md)
