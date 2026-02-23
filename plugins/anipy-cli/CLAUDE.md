# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Plugin Overview

anipy-cli plugin — Claude Code interface for anime streaming/downloading via anipy-cli on Windows.

## Architecture

```
commands/anipy-cli.md     → /anipy-cli command (entry point, execution logic)
skills/anipy-cli/
  SKILL.md                → Operational knowledge (CLI flags, player routing, config)
  references/
    setup-guide.md        → Dependency install procedures (uv, scoop, mpv, vlc)
    troubleshooting.md    → Windows error catalog (encoding, PATH, PowerShell)
```

**Command** parses natural language → maps to `anipy-cli -s` invocation.
**Skill** provides the knowledge base the command references. Also auto-triggers on phrases like "play anime".

## Key Design Decisions

- **Execution first, fix on failure** — never check deps upfront. Run the command; if it fails, diagnose and repair with user consent via `AskUserQuestion`.
- **Non-interactive only** — all anipy-cli calls use `-s` flag. Interactive modes (`-H`, `-S`, `-A`, `-M`) hang in Claude Code's Bash tool.
- **`PYTHONIOENCODING=utf-8`** prefix on every command — prevents Windows cp1252 encoding crashes.
- **Player priority**: mpv > vlc > mpvnet. Config updated via `anipy-cli --config-path`.
- **PowerShell 7 (`pwsh.exe`)** over `powershell.exe` — Windows 11 security module is often broken.

## Commands

| Command | Description |
|---------|-------------|
| `/anipy-cli [query]` | Natural language anime operations |

## Version

Current: v0.1.0 — Windows only, allanime provider.
