# pso test

> Run each service's own tests (npm test / mvnw test); fails if any fail

Run each service's own tests (npm test / mvnw test); fails if any fail.

## Usage

```text
pso test [--service name] [--json]
```

## Flags

`--service`, `--json`.

## Examples

```bash
pso test
pso test --service catalog
```

## Failure cases

- Services without tests are reported as `no-tests`, never as passed.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).
