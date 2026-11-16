# pso make:notifier

> Generate a typed Spring publisher for a channel event (a record and a service on PsoChannelPublisher; --durable stores the event in the transactional outbox)

Generate a typed Spring publisher for a channel event (a record and a service on PsoChannelPublisher; --durable stores the event in the transactional outbox).

## Usage

```text
pso make:notifier <name> --service <spring-service> --channel <candidate.{candidateId}> --event <candidate.updated> --fields "candidateId:string,status:string" [--durable] [--dry-run] [--force] [--json]
```

## Flags

`--service`, `--channel`, `--event`, `--fields`, `--durable`, `--dry-run`, `--force`, `--json`.

## Examples

```bash
pso make:notifier candidate --service recruitment-service --channel "candidate.{candidateId}" --event candidate.updated --fields "candidateId:string,status:string" --dry-run
pso make:notifier candidate --service recruitment-service --channel "candidate.{candidateId}" --event candidate.updated --fields "candidateId:string,status:string" --durable
```

## What it changes

- Writes the event record (events/) and a service that publishes it with `PsoChannelPublisher`; `--durable` selects the transactional outbox.

## Failure cases

- The service must already use Channels (`pso channels:enable`); every channel parameter must be one of the fields. The orchestrator must register a channel listing this service in its `publishers`.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [orchestrator/channels](orchestrator/channels.md)
