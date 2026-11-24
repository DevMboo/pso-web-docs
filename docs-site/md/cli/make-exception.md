# pso make:exception

> Generate an application exception in shared/exception (layered Spring service), answered as an RFC 9457 problem

Generate an application exception in shared/exception (layered Spring service), answered as an RFC 9457 problem.

## Usage

```text
pso make:exception <name> [--service <name>] [--dry-run] [--force] [--json]
```

## Flags

`--service`, `--dry-run`, `--force`, `--json`.

## Examples

```bash
pso make:exception out-of-stock --service catalog
```

## What it changes

- Writes a `PsoProblemException` subclass in `shared/exception`: an RFC 9457 problem whose detail is a sentence you write.

## Failure cases

- `needs the layered structure`: see `pso structure:inspect`.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [architecture/java-microservice](architecture/java-microservice.md)
