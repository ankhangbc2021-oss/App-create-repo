## Summary

<!-- What release version is this PR for? What changed in the manifest? -->

- **Version:** vX.Y.Z
- **Replaces:** vA.B.C

## Checklist

- [ ] `releases/latest/VERSION` matches `latest.json` → `version`
- [ ] All platform entries have non-empty `signature` values
- [ ] Download URLs point to the correct GitHub Release tag
- [ ] `checksums.txt` matches the built artifacts
- [ ] `release-notes.md` and `CHANGELOG.md` are updated
- [ ] Previous `latest/` snapshot archived under `releases/archive/<old-version>/`
- [ ] No private keys, `.env` files, or binaries committed

## Test plan

- [ ] Installed previous version detects the new update
- [ ] Update downloads and installs successfully
- [ ] SHA-256 hashes verified against `checksums.txt`
