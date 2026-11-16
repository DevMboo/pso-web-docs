# pso frontend:nginx

> Print a same-origin nginx configuration for a site root (FEI, remotes, API kept apart; restrictive Content-Security-Policy). The managed adapter of the runtime agent generates exactly these locations when the shell is deployed (`pso frontend:declare`), so this is a preview for review

Print a same-origin nginx configuration for a site root (FEI, remotes, API kept apart; restrictive Content-Security-Policy). The managed adapter of the runtime agent generates exactly these locations when the shell is deployed (`pso frontend:declare`), so this is a preview for review.

## Usage

```text
pso frontend:nginx --site <dir> [--base-path /commerce/] [--server shop.example.com] [--api /api] [--api-port 3000] [--route /catalog/sku/:id,/files/*] [--connect-src https://idp.example.com]
```

## Flags

`--site`, `--base-path`, `--server`, `--api`, `--api-port`, `--route`, `--connect-src`.

## Examples

```bash
pso frontend:nginx --site /srv/pso/sites/shop --base-path /commerce/
```

## What it changes

- Prints a same-origin nginx configuration for the site root: shell, remotes and API kept apart, immutable caching for versioned files, no-store for pointers and a restrictive Content-Security-Policy. It is for review; PSO does not apply it.

## Failure cases

- `Invalid --api`: a single path segment that is not under the shell's base path.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [frontend/releases](frontend/releases.md)
- [frontend/security](frontend/security.md)
