# pso agent:enroll

> Enroll this machine with a single-use token an administrator created (the token is read from standard input or a hidden prompt, never from an argument)

Enroll this machine with a single-use token an administrator created (the token is read from standard input or a hidden prompt, never from an argument).

## Usage

```text
pso agent:enroll <connection> [--json]
```

## Flags

`--json`.

## Examples

```bash
pso agent:enroll prod
```

## What it changes

- Reads a single-use token from standard input or a hidden prompt and enrolls a fresh client key; the token is spent.

## Failure cases

- `enrollment refused`: the token is invalid, expired or already used (the server does not say which). Repeated failures lock the source out.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [cloud/agent-and-enrollment](cloud/agent-and-enrollment.md)
