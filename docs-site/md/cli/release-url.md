# pso release:url

> Where the deployed application can be reached, from the deployment metadata and the REAL nginx file; --verify measures each layer independently (process, local health, proxy, this machine, TLS, optional SSH tunnel). COMPLETED is not proof of public access

Where the deployed application can be reached, from the deployment metadata and the REAL nginx file; --verify measures each layer independently (process, local health, proxy, this machine, TLS, optional SSH tunnel). COMPLETED is not proof of public access.

## Usage

```text
pso release:url <application> [--verify] [--tunnel] [--ca-file <pem>] [--save-ca] [--json]
```

## Flags

`--verify`, `--tunnel`, `--ca-file`, `--save-ca`, `--json`.

## Examples

```bash
pso release:url shop-gateway
pso release:url shop-gateway --verify
pso release:url shop-gateway --verify --tunnel --ca-file ./ca.pem --save-ca
```

## What it changes

- Reads the nginx file PSO manages on the server and lists every URL of the application (public, redirect, private loopback). With --verify it measures process, local health, proxy, reachability from this machine, TLS and optionally an authenticated SSH tunnel, each on its own.

## Failure cases

- A COMPLETED deployment is not proof of public access: a silent timeout from this machine with a healthy server is reported as a hypothesis (filtering on the path), never as a named cause.
- A private-CA certificate needs --ca-file; verification is never disabled.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).
