# pso release:nginx

> The real nginx configuration of the server: files, enabled links, effective includes, virtual hosts, upstreams, TLS paths and which files PSO manages. Edits are limited to PSO-managed files, are linted, checked against concurrent changes, validated by nginx -t and restored automatically on failure

The real nginx configuration of the server: files, enabled links, effective includes, virtual hosts, upstreams, TLS paths and which files PSO manages. Edits are limited to PSO-managed files, are linted, checked against concurrent changes, validated by nginx -t and restored automatically on failure.

## Usage

```text
pso release:nginx <application> [--show <file>] [--validate <file> [--content <local-file>]] [--apply <file> --content <local-file> --base <sha256> --yes] [--enable <file> --yes] [--disable <file> --yes] [--backups] [--restore <backup-id> --yes] [--json]
```

## Flags

`--show`, `--validate`, `--content`, `--apply`, `--base`, `--yes`, `--enable`, `--disable`, `--backups`, `--restore`, `--json`.

## Examples

```bash
pso release:nginx shop-gateway
pso release:nginx shop-gateway --show /etc/nginx/pso.d/pso-shop-gateway.conf
pso release:nginx shop-gateway --backups
```

## What it changes

- Without options lists the nginx files PSO can see (available, enabled, effective, PSO-managed or external). --apply, --enable, --disable and --restore change only PSO-managed files, after a lint, a base-hash check, nginx -t and an automatic restore on failure.

## Failure cases

- `the file changed after you read it`: read it again (--show) and redo the edit.
- An external file is read-only. The next deployment of the application overwrites manual edits to its managed file.
- In Git Bash on Windows set MSYS_NO_PATHCONV=1 so a /etc/... path is not rewritten.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).
