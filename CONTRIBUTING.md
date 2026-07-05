# Contributing to Bulk Repository Creator Release Manifests

Thank you for helping maintain release metadata for Bulk Repository Creator. This guide covers how to publish a new version, update the Tauri updater manifest, and keep the archive in sync.

> **Note:** Application source code lives in [ankhangbc2021-oss/App-create-repo](https://github.com/ankhangbc2021-oss/App-create-repo). This repository tracks release artifacts metadata only.

---

## Table of Contents

- [Who Should Contribute](#who-should-contribute)
- [Prerequisites](#prerequisites)
- [Release Workflow](#release-workflow)
- [Updating latest.json](#updating-latestjson)
- [Generating Checksums](#generating-checksums)
- [Archiving a Release](#archiving-a-release)
- [Publishing to GitHub Releases](#publishing-to-github-releases)
- [Pull Request Checklist](#pull-request-checklist)
- [Reporting Issues](#reporting-issues)

---

## Who Should Contribute

Release manifest changes are typically made by **maintainers** who have:

- Access to the Tauri signing private key (`TAURI_SIGNING_PRIVATE_KEY`)
- Permission to create GitHub Releases on the main application repository
- Built and tested release artifacts for the target platform

If you want to contribute to the application itself (features, bug fixes, translations), open issues or pull requests on the [main repository](https://github.com/ankhangbc2021-oss/App-create-repo).

---

## Prerequisites

Before cutting a release, confirm you have:


| Requirement          | Details                                                                            |
| -------------------- | ---------------------------------------------------------------------------------- |
| **Built artifacts**  | `Bulk Repository Creator.exe` and `Bulk Repository Creator.msi` for Windows x86_64 |
| **Tauri signer key** | Private key used to sign update bundles; public key is embedded in the app         |
| **Tested build**     | Installer runs cleanly on a clean Windows machine or VM                            |
| **Version number**   | Semantic version agreed upon (e.g. `1.2.0`)                                        |


Environment variables commonly used during the build (on the main app repo):

```bash
TAURI_SIGNING_PRIVATE_KEY="<your-private-key>"
TAURI_SIGNING_PRIVATE_KEY_PASSWORD="<optional-password>"
```

Never commit private keys, passwords, or `.env` files to this repository.

---

## Release Workflow

Follow these steps in order for every new version.

### 1. Archive the current `latest/` snapshot

Before overwriting `releases/latest/`, copy the entire directory to the archive:

```bash
# Example: archiving v1.1.0 before shipping v1.2.0
cp -r releases/latest releases/archive/v1.1.0
```

On Windows (PowerShell):

```powershell
Copy-Item -Recurse releases\latest releases\archive\v1.1.0
```

Each archive folder should be **immutable** once published. Do not edit archived manifests after a release ships.

### 2. Build and sign release artifacts

Build the application on the main repository using your standard release pipeline. Capture:

- The installer files (`.exe`, `.msi`)
- The minisign **signature** string for each platform artifact used by the Tauri updater

### 3. Update `releases/latest/`

Edit the following files:


| File               | Action                                                                      |
| ------------------ | --------------------------------------------------------------------------- |
| `VERSION`          | Set to the new version (e.g. `1.2.0`)                                       |
| `release-notes.md` | Write user-facing changelog for this version                                |
| `checksums.txt`    | Add SHA-256 hashes for all artifacts                                        |
| `latest.json`      | Update `version`, `pub_date`, `notes`, and per-platform `url` + `signature` |
| `README.md`        | Update version, build date, and release tag references                      |


Use [releases/latest/release-notes.md](releases/latest/release-notes.md) as a template for structure:

```markdown
# Bulk Repository Creator v1.2.0
**Release Date:** YYYY-MM-DD

## Improvements & Bug Fixes
* ...

## Known Issues
* ...
```

### 4. Update the project changelog

Add an entry to [CHANGELOG.md](CHANGELOG.md) summarizing the release.

### 5. Open a pull request

Submit your changes for review using the [pull request checklist](#pull-request-checklist) below.

### 6. Publish GitHub Release

After the PR is merged, upload artifacts to the main app repository (see [Publishing to GitHub Releases](#publishing-to-github-releases)).

---

## Updating latest.json

The Tauri updater manifest must be valid JSON with this schema:

```json
{
  "version": "1.2.0",
  "pub_date": "2026-07-05T12:00:00Z",
  "notes": "Release v1.2.0. Check release-notes.md for more details.",
  "platforms": {
    "windows-x86_64": {
      "url": "https://github.com/ankhangbc2021-oss/App-create-repo/releases/download/v1.2.0/Bulk%20Repository%20Creator.exe",
      "signature": "<minisign-signature-from-tauri-signer>"
    }
  }
}
```

### Field reference


| Field                   | Required | Description                                                   |
| ----------------------- | -------- | ------------------------------------------------------------- |
| `version`               | Yes      | Semver string; must be **greater** than the installed version |
| `pub_date`              | Yes      | ISO 8601 UTC timestamp of the release                         |
| `notes`                 | Yes      | Short summary shown in the updater UI                         |
| `platforms`             | Yes      | Map of target triple → download metadata                      |
| `platforms.*.url`       | Yes      | HTTPS URL to the signed update bundle                         |
| `platforms.*.signature` | Yes      | Minisign signature; **must not be empty** for production      |


### Platform identifiers

Currently supported:


| Key              | Target         |
| ---------------- | -------------- |
| `windows-x86_64` | 64-bit Windows |


Add new platform keys only when builds are available and the app is configured to check them.

### Common mistakes

- **Empty signature** — The updater will refuse to install. Always sign production builds.
- **Wrong URL** — Must match the exact GitHub Release tag and filename (URL-encode spaces as `%20`).
- **Stale version** — `version` in `latest.json` must match `VERSION` and the GitHub Release tag.
- **Mismatched checksums** — Regenerate `checksums.txt` after any artifact change.

---

## Generating Checksums

After building installers, compute SHA-256 hashes and write them to `releases/latest/checksums.txt`:

**PowerShell**

```powershell
Get-FileHash "Bulk Repository Creator.exe" -Algorithm SHA256
Get-FileHash "Bulk Repository Creator.msi"  -Algorithm SHA256
```

**Linux / macOS**

```bash
shasum -a 256 "Bulk Repository Creator.exe"
shasum -a 256 "Bulk Repository Creator.msi"
```

Format (lowercase or uppercase hex is fine; stay consistent within a file):

```
<SHA256_HASH>  Bulk Repository Creator.exe
<SHA256_HASH>  Bulk Repository Creator.msi
```

---

## Archiving a Release

The `releases/archive/` directory preserves historical manifests for auditing and rollback reference.

Rules:

1. **One folder per version** — e.g. `releases/archive/v1.1.0/`
2. **Copy, don't move** — Archive before updating `latest/`
3. **Never mutate archives** — If a published manifest had an error, ship a patch version instead
4. **Include all files** — `latest.json`, `VERSION`, `checksums.txt`, `release-notes.md`, `README.md`, `LICENSE`

---

## Publishing to GitHub Releases

On [ankhangbc2021-oss/App-create-repo](https://github.com/ankhangbc2021-oss/App-create-repo):

1. Create a new release tagged `v<version>` (e.g. `v1.2.0`)
2. Paste the contents of `releases/latest/release-notes.md` into the release description
3. Upload release assets:
  - `Bulk Repository Creator.exe`
  - `Bulk Repository Creator.msi`
4. Optionally attach manifest files (`latest.json`, `checksums.txt`) for transparency
5. Publish the release **before** users receive update notifications

Confirm the `url` in `latest.json` resolves to the uploaded asset (open the link in a browser).

---

## Pull Request Checklist

Before requesting review, verify:

- `releases/latest/VERSION` matches `latest.json` → `version`
- `releases/latest/latest.json` has a non-empty `signature` for every platform
- Download URLs use the correct tag and URL-encoded filenames
- `checksums.txt` hashes match the built artifacts
- `release-notes.md` describes user-visible changes
- `CHANGELOG.md` has a new entry for this version
- Previous `latest/` snapshot is archived under `releases/archive/<old-version>/`
- No secrets, private keys, or build artifacts committed to git

---

## Reporting Issues


| Issue type                   | Where to report                                                                                  |
| ---------------------------- | ------------------------------------------------------------------------------------------------ |
| Updater / manifest problems  | [Issues on this repository](https://github.com/ankhangbc2021-oss/App-create-repo/issues)         |
| Application bugs or features | [Issues on the main app repository](https://github.com/ankhangbc2021-oss/App-create-repo/issues) |
| Security vulnerabilities     | See [SECURITY.md](SECURITY.md) — **do not** open public issues                                   |


When filing an updater issue, include:

- Installed app version
- Contents of `latest.json` (or link to the manifest)
- Error message from **Settings → System → Check for Updates**
- Operating system and architecture

---

## Code of Conduct

Be respectful and constructive. Maintainers reserve the right to close contributions that are incomplete, unsigned, or not tested.

---

## Questions?

Open a [discussion or issue](https://github.com/ankhangbc2021-oss/App-create-repo/issues) if anything in this guide is unclear. Keeping the release process documented and repeatable helps every user get safe, verified updates.