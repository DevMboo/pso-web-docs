# pso channels:enable

> Connect a service to PSO Channels through Dispatch: for a Spring service the starter, pso.channels and the channels.publish event; for the orchestrator @pso/channels and the Dispatch consumer that delivers events to browsers

Connect a service to PSO Channels through Dispatch: for a Spring service the starter, pso.channels and the channels.publish event; for the orchestrator @pso/channels and the Dispatch consumer that delivers events to browsers.

## Usage

```text
pso channels:enable [--service <name>] [--dry-run] [--json]
```

## Flags

`--service`, `--dry-run`, `--json`.

## Examples

```bash
pso channels:enable --service gw --dry-run
pso channels:enable --service gw
pso channels:enable --service recruitment-service
```

## What it changes

- Orchestrator: adds `@pso/channels` and a Dispatch consumer that hands `channels.publish` events to the channel hub.
- Spring service: adds the channels starter, `pso.channels.enabled` and the `channels.publish` event declaration.

## Failure cases

- Dispatch must be enabled with a provider for events to travel (`pso dispatch:install`); durable events also need the outbox. Running it again changes nothing.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [orchestrator/channels](orchestrator/channels.md)
- [dispatch/installation](dispatch/installation.md)
