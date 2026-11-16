# pso connection:tunnel

> Open a private SSH tunnel from a loopback port here to a loopback port on the server (for example a service's actuator); never binds publicly, runs until Ctrl+C

Open a private SSH tunnel from a loopback port here to a loopback port on the server (for example a service's actuator); never binds publicly, runs until Ctrl+C.

## Usage

```text
pso connection:tunnel <name> --remote-port <port> [--local-port <port>]
```

## Flags

`--remote-port`, `--local-port`.

## Examples

```bash
pso connection:tunnel prod --remote-port 20001 --local-port 9001
```

## What it changes

- Forwards a loopback port here to a loopback port on the server through the authenticated SSH connection; never binds publicly.

## Failure cases

- The tunnel is for private access to services; the management plane has no network port, so it is never what you tunnel to.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [cloud/connect-a-server](cloud/connect-a-server.md)
