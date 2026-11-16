# pso environment:network

> Every listening socket of the server, who owns it and whether it is meant to be public. Flags internal infrastructure (RabbitMQ, Redis, PostgreSQL, Kafka, PSO services) on all interfaces as potentially unsafe and prints a remediation plan; it changes nothing. --verify adds a TCP check from this machine

Every listening socket of the server, who owns it and whether it is meant to be public. Flags internal infrastructure (RabbitMQ, Redis, PostgreSQL, Kafka, PSO services) on all interfaces as potentially unsafe and prints a remediation plan; it changes nothing. --verify adds a TCP check from this machine.

## Usage

```text
pso environment:network <application> [--verify] [--json]
```

## Flags

`--verify`, `--json`.

## Examples

```bash
pso environment:network shop-gateway
pso environment:network shop-gateway --verify --json
```

## What it changes

- Reads every listening socket of the server and assesses it against the environment's exposure policy; prints findings and a remediation plan. Changes nothing.

## Failure cases

- Exit 1 when a listener is potentially unsafe. A listener on all interfaces is potentially, not necessarily, reachable: firewalls and cloud rules decide, and the server cannot see them. --verify adds a TCP check from this machine.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).
