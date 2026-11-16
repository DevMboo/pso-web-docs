# Downloads

> PSO Web installer availability, Windows requirements, recipient enrollment and release verification.

> Status: PARTIALLY implemented — read the limitations.

**PSO Web — Cloud Ecosystem Framework**

**One ecosystem. Independent teams. Unified architecture.**

**Build independently. Integrate seamlessly. Operate securely.**

> [!NOTE]
> **Private evaluation pre-release — security assessment incomplete.** The installer is available only to GitHub users with access to the private release repository. This publication is not final security approval or unrestricted distribution approval.

## PSO Installer — Windows x64

| Release detail | Value |
| --- | --- |
| Version | `0.1.0-alpha.1` |
| Platform | Windows 10/11 x64 |
| Installer size | 8.8 MB (8,801,368 bytes) |
| Runtime requirement | Node.js 24 or newer on `PATH` |
| Distribution | Encrypted private evaluation |
| Security assessment | Incomplete |
| Authenticode | Not signed |

:::download-button url="https://github.com/DevMboo/pso-web-releases/releases/download/v0.1.0-alpha.1/PSO-Web-0.1.0-alpha.1-win-x64.exe" label="Download PSO Web — Windows x64"

The [PSO Web 0.1.0 Alpha 1 private release page](https://github.com/DevMboo/pso-web-releases/releases/tag/v0.1.0-alpha.1) provides checksums, signature verification materials, installation instructions and release notes. You must sign in to GitHub **and** have access to `DevMboo/pso-web-releases` to view or download its assets. The public documentation does not proxy the private file or contain GitHub credentials.

**Downloading does not authorize installation.** This candidate is encrypted for the existing validation recipient only. It installs for the same Windows user who enrolled as that recipient; administrator rights are not required. Other developers must complete recipient enrollment and receive a future installer encrypted for their public keys. Spring Boot projects additionally need JDK 21 or newer and Maven prerequisites described in [Installation](getting-started/installation.md).

## Recipient enrollment

When invited to the preview, obtain the official `pso-recipient.mjs` utility and release instructions from the private release or maintainer. On your own Windows account, run:

```powershell
node pso-recipient.mjs init --label "Your Name"
```

Send **only** the generated `.enrollment.json` public enrollment file to the maintainer. Keep the private key on your machine. Confirm your public-key fingerprint with the maintainer through a separate channel. The maintainer must create a new installer encrypted for your key; an existing candidate cannot authorize you after it was built. The [installation lifecycle guide](devops/distribution-and-installation.md) explains the authorization model.

## Verify the release before installation

Obtain the installer, `SHA256SUMS`, `SHA256SUMS.sig`, `release-signing-key.public.pem`, and `release-manifest.json` from the private release. Confirm the signing-key fingerprint through a channel separate from the downloaded files. Do not treat a fingerprint inside the same download as independent trust.

In PowerShell, compare the installer's SHA-256 with its entry in `SHA256SUMS`:

```powershell
Get-FileHash .\PSO-Web-0.1.0-alpha.1-win-x64.exe -Algorithm SHA256
Get-Content .\SHA256SUMS
```

The expected installer SHA-256 is `fc273f6e216c0f4b901c8eec4c28a2db20341bc5f095fd0dc4560e2638523ca2`. Verify the detached Ed25519 signature using a trusted verification utility and the independently confirmed public key. Pin that signer with `node pso-recipient.mjs pin-signer --fingerprint <confirmed-fingerprint>` before installation. If a checksum, signature, or fingerprint differs, stop and contact the maintainer. This candidate is not Authenticode-signed, so Windows may show an unknown publisher warning or an application-control policy may block it; do not bypass organizational policy.

## Install, update and repair

Only the existing validation recipient may install this candidate after the maintainer confirms that identity and local policy permits it. Other recipients must wait for a future installer built for their public keys. Run an authorized installer as the enrolled Windows user, then check the CLI and installed files:

```powershell
pso doctor
pso integrity check --full --root C:\Tools\pso
```

Use `pso repair --root C:\Tools\pso` to restore damaged files from the authenticated package. Use `pso update check`, `pso update install`, and `pso update rollback` as documented in [Signed packages and installation lifecycle](devops/distribution-and-installation.md). These commands require an installed, authorized release; they are not a way to obtain the preview before approval.
