# anipy-cli

**Version 0.1.0**

Claude Code plugin that provides a natural language interface for anipy-cli — search, play, download, and manage anime from the terminal on Windows.

## Features

- **Natural language mapping**: "play frieren ep 1 sub" → `anipy-cli -s "frieren:1:sub"`
- **Self-healing dependencies**: Installs uv, anipy-cli, mpv/vlc on failure — never upfront
- **Player routing**: mpv > vlc > mpvnet, auto-configured in `config.yaml`
- **Non-interactive mode**: All commands use `-s` flag for Claude Code compatibility
- **Download & binge modes**: Episode ranges, quality selection, batch downloads
- **Windows-specific**: Handles encoding, PATH, PowerShell quirks

## Prerequisites

- Windows with Git Bash
- uv (auto-installed if missing)
- mpv or VLC (auto-installed if missing)

## Installation

```bash
/plugin install anipy-cli@kana-code-plugins
```

## Usage

```bash
# Play anime
/anipy-cli play frieren ep 1 sub

# Download episodes
/anipy-cli download one piece 1-10 dub

# Binge watch
/anipy-cli binge spy x family 1-5 dub

# View history
/anipy-cli show history

# Change player
/anipy-cli change player to vlc
```

Or trigger via natural language: "play anime steins gate episode 5"

## How It Works

```
User: "play frieren ep 1 sub"
  → /anipy-cli command parses intent
  → Maps to: PYTHONIOENCODING=utf-8 anipy-cli -s "frieren:1:sub" 2>&1
  → If fails: diagnose → repair dependency → retry
```

### Dependency Repair Chain

Only triggered on failure, never upfront. Each install requires user confirmation.

1. **uv** — Python package manager (installs via PowerShell)
2. **anipy-cli** — Anime CLI tool (installs via `uv tool install`)
3. **Video player** — mpv via scoop, or VLC
4. **Config** — Updates `player_path` in anipy-cli config

## Architecture

| File | Purpose |
|------|---------|
| `commands/anipy-cli.md` | Entry point — intent parsing, execution, error recovery |
| `skills/anipy-cli/SKILL.md` | CLI knowledge — flags, modes, player routing, config |
| `skills/anipy-cli/references/setup-guide.md` | Dependency install procedures |
| `skills/anipy-cli/references/troubleshooting.md` | Windows error catalog and solutions |

## Troubleshooting

**Command hangs:**
- Ensure `-s` flag is used (interactive modes hang in Claude Code)
- Check if `preferred_type` is set in config (null triggers interactive prompt)

**Player not found:**
- Run `where.exe mpv` or `where.exe vlc` to check PATH
- Update `player_path` in config via `anipy-cli --config-path`

**Encoding errors:**
- All commands are prefixed with `PYTHONIOENCODING=utf-8` automatically

**PowerShell errors:**
- Use `pwsh.exe` (PowerShell 7) instead of `powershell.exe`
- Install PowerShell 7 from Microsoft Store if needed

## License

MIT
