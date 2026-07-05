# Changelog

All notable releases of Bulk Repository Creator are documented here.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [1.1.0] - 2026-07-05

### Added
- Vietnamese localization across all UI components
- GitHub real-time repository synchronization with sorting and manual refresh
- History page with advanced sorting, searching, filtering, and native browser links
- Native Tauri automatic updates

### Changed
- Improved empty states, hover interactions, loading states, and responsive layouts
- Settings now persist via SQLite through the background Node.js service

### Fixed
- Private repository visibility and validation through Octokit

### Release artifacts
- `Bulk Repository Creator.exe` — SHA-256: `24C472812F6E3A39E5B38E4F801838D93C8843969BDF68F87CBA93EA8437F421`
- `Bulk Repository Creator.msi` — SHA-256: `0AF7864BDB995B12A21AB8AA23C2D7A2B61A2FBB30832EFE7A9CED3CE133D920`

[Full release notes](releases/archive/v1.1.0/release-notes.md) · [Manifest](releases/archive/v1.1.0/latest.json)

---

## [1.0.0] - 2026-07-05

Initial public release of Bulk Repository Creator.

### Added
- Bulk GitHub repository creation workflow
- Repository list view with GitHub synchronization
- History tracking for created repositories
- Settings management with SQLite persistence
- Tauri-based desktop shell with auto-updater support

[Full release notes](releases/archive/v1.0.0/release-notes.md)

---

[1.1.0]: https://github.com/ankhangbc2021-oss/App-create-repo/releases/tag/v1.1.0
[1.0.0]: https://github.com/ankhangbc2021-oss/App-create-repo/releases/tag/v1.0.0
