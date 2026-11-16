# pso release:verify

> Ask the environment's SERVER whether a signed manifest is authentic (a trusted, unrevoked signer) and integral (it names exactly the release), reporting the two separately

Ask the environment's SERVER whether a signed manifest is authentic (a trusted, unrevoked signer) and integral (it names exactly the release), reporting the two separately.

## Usage

```text
pso release:verify <environment> --signed <file> [--json]
```

## Flags

`--signed`, `--json`.

## Examples

```bash
pso release:verify production --signed production.release.signed.json
```

## Failure cases

- Reports authenticity (a trusted, unrevoked, unexpired signer) and integrity (the manifest names exactly the release) separately; only `verified` authenticity exits 0.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).
