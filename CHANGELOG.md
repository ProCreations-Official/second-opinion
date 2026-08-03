# Changelog

## GitHub Release 1.0 — 2026-08-03

Second Opinion's first GitHub release adds a cross-platform manager for delegated coding-agent work.

### Added

- Lightweight native task-manager window for macOS, Windows, and Linux, opened by default for background tasks.
- Terminal manager with the same core task controls.
- `--manager app|terminal|none`, `--ui`, `--no-window`, and `SECOND_OPINION_MANAGER` preferences.
- Live task output, queued steering messages, next-turn model changes, stop, retry, archive, restore, and task creation.
- Managed turn workers that keep each task on its specialized Codex, Claude Code, OpenCode, Grok Build, or Antigravity CLI harness.
- Cross-platform GitHub Actions CI on Python 3.10 and 3.13.

### Isolation and performance

- No browser, Electron runtime, local server, or always-on daemon.
- Normal foreground CLI commands do not load the GUI toolkit or start a manager.
- The manager only reads and writes Second Opinion job records and does not edit coding-agent applications or settings.
- Displayed log data is capped to keep long-running task views responsive.

The internal CLI version advances from 1.4.0 to 1.5.0 so existing installations can detect this update. `1.0` is the GitHub release version requested for this milestone.
