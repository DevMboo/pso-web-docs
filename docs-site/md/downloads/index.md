# Downloads

> PSO Web installer availability, Windows requirements, recipient enrollment and release verification.

> Status: PARTIALLY implemented — read the limitations.

**PSO Web — Cloud Ecosystem Framework**

**One ecosystem. Independent teams. Unified architecture.**

**Build independently. Integrate seamlessly. Operate securely.**

> [!NOTE]
> **Private Developer Preview — Security validation in progress.** The protected PSO Installer is not available for download. No release asset is published yet.

## PSO Installer — Windows x64

The planned private installer supports Windows 10 or 11 x64. It requires Node.js 24 or newer on `PATH` and installs for the same Windows user who enrolled as a recipient; administrator rights are not required. Spring Boot projects additionally need JDK 21 or newer and Maven prerequisites described in [Installation](getting-started/installation.md).

The future release location is [PSO Web releases](https://github.com/DevMboo/pso-web-releases/releases). That repository is private; access to its page does not imply access to release assets. The installer will appear there only after security approval. No public proxy serves private release files.

## Recipient enrollment

When invited to the preview, obtain the official `pso-recipient.mjs` utility and release instructions from the maintainer. On your own Windows account, run:

```powershell
node pso-recipient.mjs init --label "Your Name"
```

Send **only** the generated `.enrollment.json` public enrollment file to the maintainer. Keep the private key on your machine. Confirm your public-key fingerprint with the maintainer through a separate channel. The maintainer must create a new installer encrypted for your key; an existing candidate cannot authorize you after it was built. The [installation lifecycle guide](devops/distribution-and-installation.md) explains the authorization model.

## Verify a future release before installation

Obtain the installer, `SHA256SUMS`, `SHA256SUMS.sig`, `release-signing-key.public.pem`, and `release-manifest.json` from the approved release. Confirm the signing-key fingerprint through a channel separate from the downloaded files. Do not treat a fingerprint inside the same download as independent trust.

In PowerShell, compare the installer's SHA-256 with its entry in `SHA256SUMS`:

```powershell
Get-FileHash .\PSO-Web-<version>-win-x64.exe -Algorithm SHA256
Get-Content .\SHA256SUMS
```

Verify the detached Ed25519 signature using a trusted verification utility and the independently confirmed public key. Pin that signer with `node pso-recipient.mjs pin-signer --fingerprint <confirmed-fingerprint>` before installation. If a checksum, signature, or fingerprint differs, stop and contact the maintainer. Current preview candidates are not Authenticode-signed, so Windows may show an unknown publisher warning or an application-control policy may block them; do not bypass organizational policy.

## Install, update and repair

After the maintainer confirms that your identity is included in an approved release, run the installer as the enrolled Windows user. Then check the CLI and the installed files:

```powershell
pso doctor
pso integrity check --full --root C:\Tools\pso
```

Use `pso repair --root C:\Tools\pso` to restore damaged files from the authenticated package. Use `pso update check`, `pso update install`, and `pso update rollback` as documented in [Signed packages and installation lifecycle](devops/distribution-and-installation.md). These commands require an installed, authorized release; they are not a way to obtain the preview before approval.
