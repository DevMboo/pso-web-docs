# pso make:channel

> Generate a channel class (and, for a resource channel, a deny-by-default access provider and the event contract), register it in MODULE.pso.ts and add @pso/channels

Generate a channel class (and, for a resource channel, a deny-by-default access provider and the event contract), register it in MODULE.pso.ts and add @pso/channels.

## Usage

```text
pso make:channel <module> <name> [--pattern <recruitment | candidate.{candidateId}>] [--visibility private|public|presence|internal] [--publisher <service>[,<service>]] [--scopes a:b,c:d] [--event <type> --fields "name:string,..."] [--service <orchestrator>] [--dry-run] [--json]
```

## Flags

`--pattern`, `--visibility`, `--publisher`, `--scopes`, `--event`, `--fields`, `--service`, `--dry-run`, `--json`.

## Examples

```bash
pso make:channel recruitment updates --service gw --pattern recruitment --scopes recruitment:read --dry-run
pso make:channel recruitment candidate --service gw --pattern "candidate.{candidateId}" --publisher recruitment-service --event candidate.updated --fields "candidateId:string,status:string"
```

## What it changes

- Writes `<name>.channel.ts`, registers the class in `channels` of the module's MODULE.pso.ts and adds `@pso/channels` to package.json.
- A resource channel (parameters) also gets an access provider that refuses every subscription until you implement it, and `--event` writes the event contract DTO.

## Failure cases

- The module must exist; an invalid pattern, an unknown visibility, an internal channel without --publisher and an existing channel file are refused. Run `pso channels:validate` afterwards.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [orchestrator/channels](orchestrator/channels.md)
