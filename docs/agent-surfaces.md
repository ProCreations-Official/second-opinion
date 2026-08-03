# Agent Integration Surfaces

Second Opinion uses each agent's native local skill surface and avoids changing broader settings.

It is model agnostic: the registry describes agent surfaces, commands, skill paths, and task-routing hints. The selected agent's own CLI and user settings decide which model runs unless the user explicitly passes a `--model` override.

| Agent | Integration | Installed file |
| --- | --- | --- |
| Codex | Agent Skill | `~/.agents/skills/second-opinion/SKILL.md` |
| Claude Code | Agent Skill | `~/.claude/skills/second-opinion/SKILL.md` |
| OpenCode | Agent Skill | `~/.config/opencode/skills/second-opinion/SKILL.md` |
| Grok Build | Skill | `~/.grok/skills/second-opinion/SKILL.md` |
| Google Antigravity | Agent Skill | `~/.gemini/antigravity/skills/second-opinion/SKILL.md` |

## Runtime Delegation

The skill does not create an always-on server. When a parent agent chooses to delegate, it runs:

```bash
second-opinion ask <target-or-auto> --from <caller> --cwd "$PWD" --mode consult --background -- "Task text"
second-opinion jobs
second-opinion wait JOB_ID
```

`--background` is the default workflow taught to installed agent skills. It returns a job id immediately, writes output under `~/.second-opinion/jobs/`, and lets the parent agent keep working while the subagent runs. It also opens the native task manager by default; use `--manager terminal` or `--manager none` to choose a terminal window or no window.

`second-opinion team` extends that workflow into a true parallel worker pool. Each worker gets its own fresh native harness process, job id, role, model/effort settings, and task record. Built-in `build`, `review`, and `balanced` strategies create distinct work lenses; repeated `--role` flags define custom assignments. A team opens at most one manager window, and `second-opinion wait TEAM_ID` collects all workers.

The CLI wraps the target agent's documented non-interactive mode:

- Codex: `codex exec`
- Claude Code: `claude -p`
- OpenCode: `opencode run`
- Grok Build: `grok -p`
- Google Antigravity: `agy --print`

## Native image creation

`second-opinion image-tools --json` reports image-generation capabilities and provider-native generator choices detected from native harness skills. Codex's built-in `imagegen` skill is discovered automatically. A provider tool that is available at runtime but not represented by a discoverable skill can be declared for automatic routing with `SECOND_OPINION_IMAGE_TOOLS=agent=tool`; optional selectable generator names can be advertised with `SECOND_OPINION_IMAGE_MODELS='agent=model-a|model-b'`. An explicit `second-opinion image <agent>` request can also try a tool without a declaration.

`second-opinion image` launches the selected coding harness in work mode with a specialized artifact contract. References use `codex exec --image` and `opencode run --file` where available; Claude Code and Antigravity receive additional readable directories; every provider also receives absolute reference paths in the task. The worker must use its native image tool and save a valid raster image at the requested workspace path. Missing, empty, unchanged, or signature-invalid artifacts fail the task.

The command never calls an image API directly, installs a generator, or handles image credentials. `--model` selects the coding model supervising the task, while `--image-model` is only a request to a generator already exposed by that harness. Background image runs are ordinary managed Second Opinion tasks, so they appear in the app/TUI and support steering, retry, model changes, and archive controls.

## Manager Surfaces

`second-opinion app` opens a separate cross-platform Tk window. `second-opinion tui` offers the same core workflow in a terminal. Both discover jobs from Second Opinion's own local job directory, including teams and tasks spawned by a coding agent, and support live output, steering messages, next-turn model and reasoning-effort overrides, stop, retry, archive, restore, and task creation.

The manager never embeds into an agent application. It stays idle except for a small metadata/log poll and caps the displayed output. Closing it does not terminate active agent jobs. Follow-up messages launch through the existing task's same native harness and preserve recent output context.

## Benchmark routing tool

`second-opinion benchmarks` reads Artificial Analysis's keyless public model and Coding Agent Index pages using only the Python standard library. No API key or Artificial Analysis account is required. The CLI stores a small six-hour cache under `~/.second-opinion/cache/`, exposes JSON for manager-model routing, and includes visible source attribution. It does not send repository contents, prompts, or task records to Artificial Analysis.

## Routing Tips

- Claude Code is the best first stop for visual UI, frontend polish, responsive layout, copy tone, and product/design judgment. Use the latest/highest Claude model available to the user, often an Opus-class model when available.
- Codex is the best first stop for backend work, APIs, data flow, tests, repo-wide edits, debugging, and most general implementation tasks.
- OpenCode is fast and easy for model-flexible exploration across configured providers, but quality depends heavily on the selected model.
- Antigravity is best when Google/Gemini, Vertex, Firebase, Cloud Run, or Google-style orchestration matters.
- Grok Build is useful for broad exploration or implementation spikes when installed.

## Modes

`work` is the default. It is for narrow implementation slices and may edit files, so the parent agent should assign non-overlapping files and verify the subagent's result before integrating it.

`consult` is available with `--mode consult`. It tells the target agent to inspect and report without changing files and uses the safest available CLI policy.

## Goal Mode

Goal mode is available with `second-opinion ask --goal "..."`, but it is not the default and should not be recommended for routine delegation. For targets with a known native goal command, Second Opinion includes that target's `/goal ...` command in the subagent prompt.

Use it only when the user explicitly asks to use goals, or when the user has clearly requested a long-running delegated goal:

```bash
second-opinion ask auto --from codex --cwd "$PWD" --mode work --background --goal "Finish the migration tests and report blockers." -- "Work toward this goal in the assigned files only."
```

The parent agent still owns goal tracking. It should keep the returned job id, check `second-opinion jobs`, call `second-opinion wait JOB_ID`, verify output and edits, and avoid declaring the overall user task complete while that delegated goal is running or unresolved.

## Updating

Once installed, update the CLI and existing managed skills with:

```bash
second-opinion update
```

Use `second-opinion update --all-skills` to regenerate every supported skill file from the latest published template.

## Why Skills Instead Of Config Mutation

Second Opinion intentionally avoids editing:

- Agent model defaults
- Provider credentials
- MCP server lists
- Hooks
- Permission rules
- Project config files

This keeps the install reversible and auditable. `second-opinion uninstall --all` removes only files that contain the Second Opinion managed-file marker.
