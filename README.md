# filesystem-organiser-plugin

Claude Code plugin for organising filesystem content — local directories and cloud storage (Google Drive via rclone). Ships scan, dedupe, sort, cleanup, and rename primitives plus a provisioning skill to spin up a fresh organiser workspace.

Part of the [danielrosehill Claude Code marketplace](https://github.com/danielrosehill/Claude-Code-Plugins).

## What you get

### Primitives (always available once the plugin is installed)

**Local filesystem commands** (`/filesystem-organiser:fs-*`):
- `fs-scan` — survey a directory for clutter hotspots
- `fs-cleanup` — rule-based cleanup pass
- `fs-find-duplicates` — identify duplicate files
- `fs-flatten` — flatten over-nested directory trees
- `fs-empty-dirs` — find and remove empty directories
- `fs-rename-cleanup` — normalise filenames
- `fs-sort-by-date` / `fs-sort-by-type` — bucket by metadata
- `fs-status` — report workspace state
- `fs-init`, `fs-add-guardrail` — workspace config

**Google Drive commands** (`/filesystem-organiser:gdrive-*`):
- `gdrive-mount` / `gdrive-unmount` — rclone mount lifecycle
- `gdrive-status` — connection + mount diagnostics
- `gdrive-organize` — bucket and tidy mounted Drive content
- `gdrive-duplicates` — find duplicate cloud files
- `gdrive-inbox` — inbox-style sweep
- `gdrive-summary` — high-level Drive report
- `rclone-install` / `rclone-configure` — one-time setup helpers
- `onboard` — guided first-run

**Agent**:
- `fs-organiser` — autonomous organiser subagent for multi-step tasks

### Provisioning skill

- `/filesystem-organiser:new-workspace <name> [--variant=local|gdrive] [--local-only]`

Scaffolds a new workspace (CLAUDE.md + context/logs/notes structure), personalises it from `~/.claude/CLAUDE.md`, and (by default) creates a public GitHub repo for it.

## Pattern

Primitives live in the plugin → globally available from any cwd.
Workspace scaffolds are provisioned as **data** → no `.claude/` tree inside provisioned workspaces.
Plugin updates never touch your workspace data.

See [PLAN.md in Claude-Workspace-Reshaping-190426](https://github.com/danielrosehill/Claude-Workspace-Reshaping-190426) for the full pattern spec this plugin follows.

## Variants

- `local` (default) — scaffolds a workspace for organising a local directory. Includes guardrails and logs structure.
- `gdrive` — scaffolds a workspace for organising Google Drive via rclone. Includes scripts and context directories tuned for cloud sync.

## Install

Via the danielrosehill marketplace:

```
/plugin marketplace add danielrosehill/Claude-Code-Plugins
/plugin install filesystem-organiser
```

## License

MIT.
