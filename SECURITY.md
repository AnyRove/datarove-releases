# Security policy

## Reporting a vulnerability

Please report security problems **privately**, not in a public issue:

1. Open the [Security tab](../../security) of this repository and choose **Report a vulnerability** (GitHub private vulnerability reporting).
2. Describe the problem, the DataRove version (Settings → About), your operating system, and the steps to reproduce it.
3. Do not include real passwords, keys or database contents. Use test data.

You will get an answer as soon as possible. Fixes are released as a new version and noted in the [CHANGELOG](CHANGELOG.md). Please give us a reasonable time to ship a fix before you disclose the problem publicly.

## Supported versions

Security fixes go into the latest release. The apps check this repository for updates and verify each installer's SHA-256 before installing it, so staying on the latest version is the easiest way to get them.

## Verifying a download

Every release includes `SHA256SUMS.txt`. Compare it with the file you downloaded:

```bash
shasum -a 256 DataRove-<version>-mac-arm64.dmg          # macOS
Get-FileHash DataRove-<version>-win-x64.exe -Algorithm SHA256   # Windows PowerShell
sha256sum DataRove-<version>-linux-x86_64.AppImage       # Linux
```

Only download installers from the [Releases](../../releases) page of this repository.

## Design

How pairing, end-to-end encryption, the relay and permissions work is summarized under "Security in short" in the [README](README.md).
