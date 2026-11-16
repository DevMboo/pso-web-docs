# pso channels:validate

> Check channel declarations without running them: invalid, duplicate or ambiguous patterns, classes registered in MODULE.pso.ts that are not channels, channel classes that are not registered

Check channel declarations without running them: invalid, duplicate or ambiguous patterns, classes registered in MODULE.pso.ts that are not channels, channel classes that are not registered.

## Usage

```text
pso channels:validate [--service <orchestrator>] [--json]
```

## Flags

`--service`, `--json`.

## Examples

```bash
pso channels:validate --service gw
pso channels:validate --service gw --json
```

## Failure cases

- Static, nothing is executed: invalid, duplicate or ambiguous patterns and classes listed in `channels` that are not channels are errors (exit 1); decorated classes that are not registered are warnings because they are inactive.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [orchestrator/channels](orchestrator/channels.md)
