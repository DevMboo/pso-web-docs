# Downloads

> Current PSO Web distribution status, the available private portable preview, verification and Windows installation limits.

> Status: PARTIALLY implemented — read the limitations.

**PSO Web — Cloud Ecosystem Framework**

**One ecosystem. Independent teams. Unified architecture.**

> [!NOTE]
> **The current installed distribution is `0.2.0-rc.3`.** This documentation describes its implemented framework. Its portable candidate exists on the development machine but has not been published in the private releases repository. Documentation deployment does not authorize a software release.

## Available private preview

The latest published private download is the earlier **`0.1.0-alpha.2` portable ZIP**. It is for evaluation, not the current `0.2.0-rc.3` framework. The older `0.1.0-alpha.1` installer is encrypted for one validation recipient and should not be used as the general installation path.

| Release detail | Value |
| --- | --- |
| Published asset | `PSO-Web-0.1.0-alpha.2-win-x64-portable.zip` |
| Platform | Windows 10/11 x64 |
| ZIP size | 50.2 MB (50,196,310 bytes) |
| Node.js requirement | Bundled private runtime; no system Node.js installation |
| Access | GitHub sign-in and access to `DevMboo/pso-web-releases` |
| Security | Unsigned developer alpha; no independent publisher authentication |

:::download-button url="https://github.com/DevMboo/pso-web-releases/releases/download/v0.1.0-alpha.2/PSO-Web-0.1.0-alpha.2-win-x64-portable.zip" label="Download PSO Web — Windows x64 portable (alpha.2)"

See the [private alpha.2 release page](https://github.com/DevMboo/pso-web-releases/releases/tag/v0.1.0-alpha.2) for `PORTABLE.md`, checksums, the detached signature, SBOMs and notices. The public website does not proxy private assets or contain GitHub credentials.

## Verify before use

Download `SHA256SUMS`, `SHA256SUMS.sig` and `release-signing-key.public.pem` from the same release. For alpha.2, the ZIP must have SHA-256 `094b4cd555fd837d46688d45f82b9805b8c8c73667c6f63dc172f4f9c4c45dbe`:

```powershell
Get-FileHash .\PSO-Web-0.1.0-alpha.2-win-x64-portable.zip -Algorithm SHA256
Get-Content .\SHA256SUMS
```

The Ed25519 signature proves integrity relative to the included key. Because that key travels with the release, it does **not** independently establish the publisher's identity. Confirm a trusted fingerprint through another channel before using it as an identity claim. Stop if a checksum or signature differs.

## Current release candidate and installation limits

The local `0.2.0-rc.3` candidate is a portable Windows x64 ZIP with a bundled Node.js runtime and matching Java artifacts. Its SHA-256 is listed in the signed `SHA256SUMS` released with it; this is verification information, **not a download link or publication notice**. Only obtain it through an authorized distribution channel with its matching release materials.

For a supplied portable ZIP, follow [Installation](getting-started/installation.md): verify its checksum, extract into a writable folder, run `pso.cmd` from that folder, and optionally add its `bin` directory to your user PATH. It does not install an application or change the system Node.js. Use `pso integrity check --full`, `pso update install --package <file.psopkg>` and `pso update rollback` for maintenance. Spring services additionally need JDK 21 or newer and the matching `com.pso` Java artifacts.

The Windows installer remains **unsigned and unvalidated end to end**, so it is not a supported installation method. Do not disable or bypass Smart App Control, SmartScreen, Defender or organizational policy. See [Release integrity and signing](infrastructure/release-integrity-and-signing.md).
