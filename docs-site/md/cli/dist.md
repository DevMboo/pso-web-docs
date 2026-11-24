# pso dist

> Build, sign, encrypt, verify and extract signed .psopkg distribution packages from an explicit allowlist

Build, sign, encrypt, verify and extract signed .psopkg distribution packages from an explicit allowlist.

## Usage

```text
pso dist keygen --kind <signing|recipient> --out <dir> [--name <name>] | build [--config <file>] [--out <stage>] | pack --stage <dir> --sign-key <file> [--config <file>] [--out <file.psopkg>] [--encrypt --recipient <public.pem[,more]>] | verify <file> --trust <public.pem[,more]> [--key <recipient-private.pem>] | inspect <file> --trust <public.pem> | extract <file> --to <dir> --trust <public.pem> [--key <recipient-private.pem>] [--overwrite] | installer --platform windows --package <file.psopkg> --bootstrap <pso-bootstrap.mjs> --trust <release.public.pem> [--name <file name>] [--out <dir>] [--publisher <certificate common name> | --dev-distribution] [--runtime <node.exe> --runtime-license <file>] [--json]
```

## Flags

`--kind`, `--out`, `--name`, `--config`, `--stage`, `--sign-key`, `--encrypt`, `--recipient`, `--trust`, `--key`, `--to`, `--overwrite`, `--platform`, `--package`, `--bootstrap`, `--publisher`, `--dev-distribution`, `--runtime`, `--runtime-license`, `--json`.

## Examples

```bash
pso dist keygen --kind signing --out keys --name release
pso dist build --out stage
pso dist pack --stage stage --sign-key keys/release.private.pem --out pso.psopkg
pso dist verify pso.psopkg --trust keys/release.public.pem
```

## Failure cases

- Packing needs an explicit signing key and nothing is signed or encrypted by default. Verification without --key checks the header and signature only; it says so. The Windows installer is BLOCKED (exit 2) when Inno Setup is not installed. `dist installer` needs `--bootstrap`, exactly one `--trust` public key (a private key is refused) and no longer accepts `--stage`: the installer carries only the signed package, that one public key and the small bootstrap.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).
