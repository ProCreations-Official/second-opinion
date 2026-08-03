# Changelog

## Unreleased

### Added

- Lightweight native Markdown rendering for task output, including headings, emphasis, lists, task lists, quotes, tables, fenced and inline code, and safe clickable HTTP(S)/email links.
- A real task-manager screenshot showing Claude Code orchestrating two Grok 4.5 workers.
- `second-opinion image-tools` for machine-readable discovery of native image-generation capabilities exposed by installed coding harnesses.
- `second-opinion image` for reference-aware, foreground or managed-background image creation through a selected harness's existing native tool.
- Strict raster artifact verification and an explicit design-concept-to-implementation-worker handoff workflow.

### Changed

- The internal CLI version advances from 1.6.1 to 1.7.0 so existing installations detect the native image-generation workflow.

## GitHub Release 1.0.1 — 2026-08-03

Second Opinion now orchestrates parallel implementation and review teams, with live benchmark evidence for choosing the right model and harness.

### Added

- `second-opinion team` for 1–32 genuinely parallel native-harness workers with `build`, `review`, `balanced`, and custom role strategies.
- Team ids, worker roles and indices, `jobs --team TEAM_ID`, whole-team `wait`, and team-aware native/terminal manager listings.
- Provider-native `--reasoning`/`--effort` support for Codex, Claude Code, OpenCode, Grok Build, and Antigravity, including next-turn effort steering.
- `second-opinion benchmarks` for keyless Artificial Analysis model Intelligence, Coding, and Agentic indices, cost/time per task, pricing, and speed.
- Harness-specific Artificial Analysis Coding Agent Index results with coding-task cost, time, steps, and tokens.
- Six-hour benchmark caching, stale-cache fallback, offline reads, filtering, sorting, and machine-readable JSON for orchestrator models.

### Changed

- Generated skills and documentation now teach implementation worker pools and reciprocal orchestration, not only review-oriented delegation.
- The task manager displays team roles and worker positions and can change both model and reasoning effort for future turns.
- The internal CLI version advances from 1.5.0 to 1.6.0 so existing installations detect this update. `1.0.1` is the GitHub release version for this milestone.

### Privacy and performance

- Benchmark access requires no Artificial Analysis API key, account, sign-in, browser runtime, or third-party Python package.
- Benchmark requests contain no repository data, prompts, or task records and retain visible Artificial Analysis attribution.
- Teams remain opt-in local child processes; normal CLI use and coding-agent applications are unchanged.

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
