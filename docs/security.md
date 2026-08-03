# Security Model

Second Opinion is a local orchestration helper. It does not proxy prompts through a hosted service and does not store API keys.

## What It Does

- Installs a CLI in `~/.local/bin`.
- Writes one managed skill file per selected agent.
- Updates its own CLI file when `second-opinion update` downloads a newer published version.
- Runs installed agent CLIs in non-interactive mode when asked.
- Stores task metadata and logs under `~/.second-opinion/jobs/` so either local manager can show and steer delegated work.
- Opens an optional native Tk task-manager process for background tasks by default. `--manager terminal` uses a terminal window and `--manager none` opens nothing.

## What It Does Not Do

- It does not modify model settings or pin a default model.
- It does not read or write provider credentials.
- It does not install MCP servers.
- It does not install hooks.
- It does not change agent permission settings.
- It does not run a server, browser runtime, or always-on daemon. Background jobs and the optional manager are explicit local child processes started only for Second Opinion usage.
- It does not modify, inject code into, or control the Codex, Claude Code, OpenCode, Grok, or Antigravity applications.

## File Safety

Installed skill files include a managed-file marker. If a target file exists without that marker, install skips it unless `--force` is supplied.

Uninstall removes only marked files:

```bash
second-opinion uninstall --all
```

`second-opinion update` refreshes only marked, managed skill files by default. Use `--all-skills` only when you intentionally want every supported Second Opinion skill regenerated.

## Delegation Safety

`work` mode is the CLI default and may edit files. Use explicit `--mode consult` when the user wants a read-only review, plan, or risk analysis.

The generated skill tells parent agents not to pass secrets and not to create recursive delegation chains unless explicitly requested.

Steering messages and model changes are written only to the selected task record. A steering turn runs through the task's original native agent harness and its existing local authentication. Changing a model affects a future turn; it does not mutate a provider process already running. Stopping a task terminates its process group but does not revert workspace edits that the task already made.
