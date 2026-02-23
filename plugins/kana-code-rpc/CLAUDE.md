# kana-code-rpc

Claude Code plugin that displays coding activity as Discord Rich Presence.

## Spec Reference

Primary spec: `SPEC.md`

## Key Constraints

- Python 3.10+ required (uses `X | None` type hint syntax)
- `pypresence` is the only required dependency; `pyyaml` is optional (graceful fallback)
- State file (`state.json`) must always use `StateLock` for read-modify-write operations
- Sessions file (`sessions.json`) must always use `StateLock(lock_file=SESSIONS_LOCK_FILE)`
- Atomic writes via temp file + `os.replace` — never write directly to shared files
- Discord details string max ~128 chars; truncate to 120 to be safe
- Hook commands must complete within timeout (15s for start, 10s for update/stop)
- Daemon is a single shared process — never spawn duplicates (check PID file first)

## Architecture

- `scripts/presence.py` — Main entry: `start` (spawn daemon + register session), `update` (write tool to state), `stop` (unregister + kill daemon if last), `daemon` (RPC loop)
- `scripts/state.py` — Shared state: cross-platform file locking (`StateLock`), atomic JSON read/write
- `scripts/statusline.py` — Terminal statusline + feeds token/cost/duration/lines/agent/context data into `state.json`. Primary data source for Discord display.
- `hooks/hooks.json` — SessionStart/PreToolUse/SessionEnd hook definitions
- `.claude-plugin/config.yaml` — User config (display toggles, app ID, idle timeout)

### Session Management

Sessions are tracked by PID (Claude Code's process ID found by walking the parent chain).
- SessionStart hook registers Claude Code PID in `sessions.json`
- Daemon checks PID liveness every 30s via `is_process_alive()` (ctypes on Windows, os.kill on Unix)
- Dead sessions (PID no longer running) are automatically cleaned up

## Commands

- `python scripts/presence.py start` - Start session + spawn daemon
- `python scripts/presence.py stop` - End session + stop daemon if last
- `python scripts/presence.py status` - Show daemon/session status
- `python scripts/presence.py update` - Update current tool (called by hooks)

## Current Status

v0.5.0 — PID-based session management + richer display (lines, context %, agent awareness).
