[![Listed in Claude Code Repos Index](https://img.shields.io/badge/Claude%20Code%20Repos-Index-blue?style=flat-square&logo=github)](https://github.com/danielrosehill/Claude-Code-Repos-Index)

# Claude FS Organiser

A Claude Code agent workspace for scanning, cleaning, and organising your filesystem. Uses the repository as a launchpad — guardrails, scan data, cleanup logs, and slash commands all live here and persist across sessions.

## Slash Commands

| Command | Description |
|---------|-------------|
| `/init` | Startup check — loads guardrails, scan data, and logs |
| `/scan` | Scan a directory for clutter hotspots |
| `/cleanup` | Propose and execute a cleanup with approval workflow |
| `/status` | Overview of guardrails, recent scans, and cleanups |
| `/add-guardrail` | Interactively create a new guardrail rule |
| `/find-duplicates` | Checksum-based duplicate file detection |
| `/flatten` | Collapse over-nested single-child directory chains |
| `/sort-by-type` | Sort loose files into categorised folders |
| `/sort-by-date` | Sort into YY/MM/DD date-based hierarchy |
| `/empty-dirs` | Find and remove empty directories |
| `/rename-cleanup` | Fix messy filenames (Copy of, (1), Untitled, etc.) |

## Agents

| Agent | Description |
|-------|-------------|
| `fs-organiser` | Autonomous filesystem cleanup agent |

## Workspace Structure

```
├── CLAUDE.md              # Agent instructions and core rules
├── .claude/
│   ├── commands/          # Slash commands
│   └── agents/            # Autonomous agents
├── guardrails/            # User-defined rules and exclusions
├── data/                  # Scan results and filesystem analysis
├── logs/                  # Cleanup operation logs
└── notes/                 # Working notes and user guidance
```

## How It Works

1. **`/init`** — Run at the start of every session to load your guardrails and prior state.
2. **`/scan ~/Downloads`** — Identify clutter hotspots in any directory.
3. **`/cleanup ~/Downloads`** — Get a proposed cleanup plan, approve it, then execute.
4. **`/add-guardrail`** — Protect specific paths or patterns from being touched.

All operations are logged to `logs/` with reversibility notes so changes can be undone.

## Conventions

- **File naming**: `snake_case`, lowercase, under 50 characters
- **Date folders**: `YY/MM/DD` hierarchy (e.g., `25/03/31/`)
- **File date prefixes**: `YYMMDD` format (e.g., `meeting_notes_260331.md`)
- **Archives**: Four-digit year with optional quarterly subdivisions

---

*For more Claude Code projects, visit the [Claude Code Repos Index](https://github.com/danielrosehill/Claude-Code-Repos-Index).*
