# Release integrity, signing and Windows installation

> The three different guarantees a PSO distribution can carry (package integrity, cryptographic release signing, Windows code signing), what 0.2.0 has, how to install and upgrade on Windows, and what is required before an official release.

> Status: PARTIALLY implemented — read the limitations.

> [!STATUS]
> **Partial by necessity.** Package integrity (checksums, per-file hashes, SBOM, reproducible tarballs) is implemented and verified. Cryptographic release signing works with any Ed25519 key, but no official release identity exists yet. Windows code signing (Authenticode) is prepared but not done: no trusted code-signing certificate is available.

## Three different guarantees

| Guarantee | Answers | 0.2.0 release candidate |
|---|---|---|
| **Package integrity** | Is this exactly the file that was built? | `SHA256SUMS`, per-file hashes in the signed inventory of a `.psopkg`, CycloneDX SBOM, reproducible npm tarballs (two independent packs, identical integrity) |
| **Cryptographic release signing** | Did the release owner produce it? | the npm artifacts are **unsigned**; the Windows package is signed with the project's **development** key (integrity only: the key travels with the files, so it does not authenticate a publisher) |
| **Windows code signing (Authenticode)** | Has Windows verified the publisher? | **not signed**: Smart App Control, SmartScreen or enterprise policy may refuse to start the installer |

A checksum published next to the file proves integrity only as far as that page can be trusted. PSO never generates a throwaway signing identity and presents it as an official one.

## Installing on Windows

- **Portable ZIP** (recommended while the installer is unsigned): extract to a folder you can write to, run `.\pso.cmd --version`. A private Node.js runtime is bundled; nothing on the machine is changed.
- **Installer** (`PSO-Web-<version>-win-x64.exe`, Inno Setup): an experimental local candidate. It is unsigned, its real installation flow has not been validated end to end, and it is not a supported installation method for rc.3.

Maintenance of an installation (both forms):

```bash
pso integrity check --full                 # every file against the signed inventory (read-only)
pso repair --full                          # restore damaged files from the authenticated package
pso update install --package <file.psopkg> # side by side, verified, atomically activated
pso update rollback                        # back to the previous version
pso uninstall --root <dir> --yes           # standard: keeps configuration, cache and logs; your projects are never inside it
```

Upgrading from 0.1.x keeps every workspace untouched: workspaces live outside the installation. After upgrading, run `pso workspace:migrate` in each workspace (inspection first).

### Portable installation, step by step

1. Verify before extracting (PowerShell): `(Get-FileHash .\PSO-Web-<version>-win-x64-portable.zip -Algorithm SHA256).Hash` must equal the ZIP's line in `SHA256SUMS`, and `SHA256SUMS.sig` must verify with `release-signing-key.public.pem` (Ed25519). In a release candidate that key is the development key: the check proves integrity, not who published it.
2. Extract into a folder you can write to that is not `Program Files`, for example `%LOCALAPPDATA%\PSO`.
3. Run `<folder>\pso.cmd --version` and `<folder>\pso.cmd integrity check --full`.
4. Add `<folder>\bin` to your **user** PATH, open a new terminal, and confirm with `Get-Command pso -All` that it resolves to that folder (see [installed and development CLI](devops/installed-and-development-cli.md) when an npm-linked checkout answers instead).

### Windows installer limitations

The installer of `0.2.0-rc.3` is not Authenticode-signed or published as a supported installation method. A prior rc.1 unsigned build was refused by Smart App Control on the validation machine ("An Application Control policy blocked this file", Code Integrity event 3077), showing that acceptance of one unsigned file does not predict acceptance of another. PSO never asks you to switch Smart App Control, SmartScreen or Defender off. Use an **authorized portable ZIP** until the installer is signed and its installation flow validated. The rc.3 portable candidate is not yet published in the private releases repository.

## The permanent PSO release signing identity

Three keys or certificates are involved, and none replaces another:

| | Purpose | Kind | Where it is verified |
|---|---|---|---|
| Artifact checksums (`SHA256SUMS`) | the file is the one that was built | SHA-256, no key | by hand, by `pso update`, by the portable validation |
| PSO release signature | the release owner produced the package and the checksums | **Ed25519** key pair | `.psopkg` signature, `SHA256SUMS.sig`, the installation's trust store (updates signed by another key are refused) |
| Windows Authenticode | Windows knows the publisher | X.509 code-signing certificate from a public CA | Smart App Control, SmartScreen, `Get-AuthenticodeSignature` |

The development key used for release candidates (`~/.pso-psd/keys/release.private.pem`) is **not** the release identity and must not become it: it has lived on a development machine.

### Establishing it (only with the release owner's authorization)

1. **A clean, dedicated machine**, preferably offline, with full-disk encryption, no development tools beyond Node.js 24 and the released `pso`.
2. **Generate** onto encrypted removable media (for example a BitLocker To Go drive), never into a repository or a synchronised folder:

   ```powershell
   pso dist keygen --kind signing --out E:\pso-release-identity --name pso-release-2027
   ```

   The command never prints the private key and never overwrites an existing one. The private PEM is **not passphrase-protected**: its protection is the encrypted medium and the file's ACL (the owner only).
3. **Back it up**: a second encrypted medium kept in another physical place. Losing every copy means future releases cannot update existing installations.
4. **Publish the public key** `pso-release-2027.public.pem` and its fingerprint (printed by keygen) through channels users already trust (the repository's release page and a second independent channel). Record the fingerprint in the release notes of every release.
5. **Sign releases** by mounting the medium only for the build and pointing the build at it:

   ```powershell
   $env:PSO_RELEASE_SIGNING_KEY_FILE = "E:\pso-release-identity\pso-release-2027.private.pem"
   node scripts/release.mjs npm --out release-out/<version>/npm
   node scripts/psd/build-release.mjs --label <version> --dev-alpha --signing-key $env:PSO_RELEASE_SIGNING_KEY_FILE --runtime <node.exe> --runtime-license <file>
   Remove-Item Env:PSO_RELEASE_SIGNING_KEY_FILE   # then eject the medium
   ```

   The Windows pipeline has two modes today: the public development distribution (`--dev-alpha`: unencrypted, bundled Node.js) and an encrypted private preview for named recipients (`--recipients`). A public release signed with the official identity uses the first one, but its release notes and portable notice still call it an "Unsigned Developer Alpha"; that designation must be changed before the first official release.

6. **Moving installations to the official key**: an installation trusts exactly the key it was installed with, and its trust store is integrity-protected, so a package signed by the official key is refused by an installation that trusts the development key. Install the first officially signed release fresh (a new portable folder or the signed installer); workspaces are outside the installation and are not affected.
7. **Compromise**: stop signing, publish a revocation notice with the fingerprint through the same channels, generate a new identity, and ask users to install fresh.

### Windows code signing

Needs a code-signing certificate from a public certificate authority (bought by the release owner; PSO does not provide one) and `node scripts/psd/sign-installer.mjs` (signtool with the certificate in a hardware token, a PFX, or a cloud signing service). Sign the existing installer artifact; do not rebuild it to sign it.

## Before an official release

1. The permanent release identity (above), authorized by the release owner.
2. A trusted code-signing certificate and a signed installer.
3. Explicit approval to publish.

## What goes wrong

- Windows refuses to start the installer: it is not Authenticode-signed; use the portable ZIP, or ask your administrator to allow it by SHA-256.
- `pso update install` refuses a package: it is signed by another key than the installation trusts; that is the protection working.
