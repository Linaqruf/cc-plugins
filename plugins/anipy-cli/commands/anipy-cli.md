---
name: anipy-cli
description: Search, play, download, or manage anime via anipy-cli
argument-hint: [action and query, e.g. "play frieren ep 1 sub"]
allowed-tools:
  - Bash(*)
  - Read
  - Edit
  - Grep
  - Glob
  - AskUserQuestion
---

Parse the user's intent from: $ARGUMENTS

Use the anipy-cli skill for all operational knowledge — CLI flags, setup procedures, player routing, and troubleshooting.

## Execution First, Fix on Failure

Do NOT check dependencies upfront. Run the anipy-cli command directly first. Only if it fails (e.g., "command not found", "PlayerError", "not found"), then diagnose and fix:

1. If `anipy-cli` not found → check `uv`, install if needed, then `uv tool install anipy-cli`
2. If PlayerError → check mpv/vlc, install if needed, update config `player_path`
3. For each missing dependency, use `AskUserQuestion` to confirm before installing — never install silently
4. After fixing, retry the original command

## Execution

Interpret the user's natural language request and map to the appropriate anipy-cli command.

**Always prefix commands with:** `PYTHONIOENCODING=utf-8`

**Timeouts:** Use 60s for search/play commands, 120s for downloads.

**Always use `-s` flag** for non-interactive search: `anipy-cli -s "query:episode:sub/dub"`

**Mapping examples:**
- "play frieren ep 1" → `PYTHONIOENCODING=utf-8 anipy-cli -s "frieren:1:sub"`
- "play steins gate episode 3 dubbed" → `PYTHONIOENCODING=utf-8 anipy-cli -s "steins gate:3:dub"`
- "download one piece 1-10" → `PYTHONIOENCODING=utf-8 anipy-cli -D -s "one piece:1-10:sub"`
- "binge spy x family 1-5 dub" → `PYTHONIOENCODING=utf-8 anipy-cli -B -s "spy x family:1-5:dub"`
- "show history" → read `history.json` from anipy-cli's `user_files_path` (do NOT use `-H`, it is interactive and will hang)
- "what version" → `anipy-cli --version`
- "change player to vlc" → read and edit config.yaml player_path
- "set quality to 720p" → `PYTHONIOENCODING=utf-8 anipy-cli -q 720 -s "..."`

**Defaults when not specified:**
- sub/dub: `sub`
- quality: `best`
- player: whatever is in config

## Error Recovery

If a command fails:
1. Read the error message
2. Consult the anipy-cli skill's troubleshooting section
3. Attempt to fix (e.g., update config, install missing dep)
4. Retry the original command
5. If still failing, explain the issue clearly to the user

**Ignore these non-fatal messages:**
- `UserWarning: color, on_color and attrs are not supported` — yaspin TTY warning
- `SyntaxWarning: 'return' in a 'finally' block` — upstream code issue
- `EOFError: EOF when reading a line` — expected, playback still works
- Raw ANSI codes like `[34m`, `[0m` — display artifacts
