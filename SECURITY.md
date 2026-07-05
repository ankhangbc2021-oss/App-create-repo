# Security Policy

## Supported Versions

Security fixes are published only for the **current** release tracked in `releases/latest/`. Older versions archived under `releases/archive/` no longer receive updates.


| Version        | Supported |
| -------------- | --------- |
| 1.1.0 (latest) | ✅         |
| 1.0.0          | ❌         |


---

## Update Integrity

Bulk Repository Creator uses the **Tauri updater** with **minisign** signatures. Every automatic update must satisfy two checks before installation:

1. **Cryptographic signature** — The `signature` field in `latest.json` must validate against the public key embedded in the application at build time.
2. **HTTPS delivery** — Artifacts are downloaded exclusively over HTTPS from GitHub Releases.

Unsigned or tampered packages are rejected by the updater.

### Verifying downloads manually

Each release publishes SHA-256 checksums in `checksums.txt`. Compare your downloaded file before running the installer:

```powershell
Get-FileHash "Bulk Repository Creator.exe" -Algorithm SHA256
```

See [README.md](README.md#verifying-downloads) for full instructions.

---

## Reporting a Vulnerability

If you discover a security vulnerability in Bulk Repository Creator or its update infrastructure:

1. **Do not** open a public GitHub issue
2. Email the maintainer with details (include steps to reproduce, impact assessment, and affected versions)
3. Allow reasonable time for a fix before public disclosure

### What to include

- Description of the vulnerability
- Steps to reproduce
- Affected version(s)
- Potential impact (e.g. RCE, credential exposure, updater bypass)
- Proof of concept, if available

### Response expectations


| Stage              | Target                                           |
| ------------------ | ------------------------------------------------ |
| Acknowledgment     | Within 7 days                                    |
| Initial assessment | Within 14 days                                   |
| Fix or mitigation  | Depends on severity; critical issues prioritized |


---

## Maintainer Responsibilities

When publishing releases:

- Never ship builds with an **empty** `signature` in `latest.json`
- Rotate signing keys immediately if a private key is compromised
- Regenerate `checksums.txt` whenever artifacts change
- Archive previous manifests before overwriting `releases/latest/`

See [CONTRIBUTING.md](CONTRIBUTING.md) for the full release workflow.

---

## Scope

**In scope**

- Bulk Repository Creator application vulnerabilities
- Tauri updater manifest tampering or signature bypass
- Insecure artifact distribution

**Out of scope**

- Vulnerabilities in third-party services (GitHub, Octokit) unless directly exploitable through this app's integration
- Social engineering attacks
- Issues requiring physical access to an unlocked machine

---

## License

Security research conducted in good faith, following responsible disclosure, is appreciated and will not result in legal action from the maintainer.