# pso security:policy

> Show the effective authorization policy of every route and service

Show the effective authorization policy of every route and service.

## Usage

```text
pso security:policy [--json]
```

## Flags

`--json`.

## Examples

```bash
pso security:policy
```

## Failure cases

- The report describes declared policy; it does not issue or verify a token for a live request.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [security/secure-routing](security/secure-routing.md)
