# kana-code-plugins

> A Claude Code plugin marketplace with project specification, song composition, and Discord Rich Presence plugins.

## Overview

### Problem Statement
Claude Code lacks domain-specific workflows for project planning, creative composition, and activity visibility. Users need structured interview-based spec generation, Suno AI-optimized song composition, and real-time Discord status showing their coding activity.

### Solution
Three self-contained plugins extending Claude Code via its plugin system: slash commands for specification and song workflows, hooks for Discord Rich Presence integration. Distributed as a single marketplace for easy installation.

### Target Users
- **Primary**: Claude Code power users who want structured workflows and activity visibility
- **Secondary**: Suno AI musicians, Discord community members
- **Technical Level**: Developer / Technical User

---

## Plugins Included

| Plugin | Version | Type | Entry Point |
|--------|---------|------|-------------|
| `project-spec` | 4.0.0 | Commands + Skills | `/spec-writing` |
| `suno-composer` | 5.5.0 | Commands + Skills | `/suno` |
| `kana-code-rpc` | 0.5.0 | Hooks only | SessionStart / PreToolUse / SessionEnd |

---

## Plugin: project-spec (v4.0.0)

### Purpose
Front-load critical project decisions by conducting interviews and generating structured specification documents. Creates SPEC.md as the primary specification with optional SPEC/ supplements for reference material.

### Core Principle
**SPEC.md = things you READ. SPEC/ = things you LOOK UP.**

### Command

Single `/spec-writing` command with argument-based routing:

| Command | Description | Output |
|---------|-------------|--------|
| `/spec-writing` | Full project planning interview | `SPEC.md` + `CLAUDE.md` |
| `/spec-writing web-app\|cli\|api\|library` | Project spec with type pre-filled | `SPEC.md` + `CLAUDE.md` |
| `/spec-writing feature [name]` | Feature planning with gap analysis | `FEATURE_SPEC.md` or `SPEC/FEATURE-*.md` |
| `/spec-writing design [style]` | Design system interview | `DESIGN_SPEC.md` or `SPEC/DESIGN-SYSTEM.md` |
| `/spec-writing design:overhaul` | First-principles design redesign | Design spec + migration checklist |

### Key Features
- **Existing codebase detection**: Scans for config files and offers document/plan/both modes
- **Opinionated recommendations**: Lead with best options, allow override
- **Gap analysis**: For feature planning, shows specced-but-not-implemented and vice versa
- **Design audit**: For design overhaul, audits current design before first-principles redesign
- **Context7 integration**: Fetches tech stack documentation during planning
- **Supplement prompts**: Asks mid-interview if reference-heavy content should be separated
- **Session prompts**: Generates `prompt.md` for compound engineering sessions
- **Opus 4.6 optimized**: Structured prompts for adaptive thinking

### Components

```
plugins/project-spec/
├── .claude-plugin/
│   └── plugin.json              # Plugin manifest (v4.0.0)
├── commands/
│   └── spec-writing.md          # /spec-writing command with routing
├── skills/
│   └── spec-writing/
│       ├── SKILL.md             # Authoritative methodology (single source of truth)
│       ├── references/
│       │   ├── interview-questions.md
│       │   ├── output-template.md
│       │   ├── spec-type-flows.md
│       │   ├── codebase-analysis.md
│       │   ├── session-prompt-template.md
│       │   └── spec-folder-template.md
│       ├── templates/           # 11 section templates + CLAUDE.md template
│       └── examples/            # 7 example specs
├── README.md
├── CHANGELOG.md
└── LICENSE
```

### Interview Workflow
1. Check for existing specs (SPEC.md, SPEC/, PROJECT_SPEC.md)
2. Detect existing codebase (package.json, src/, config files)
3. Conduct interview (~15-20 questions in 5 phases)
   - Vision & Problem
   - Requirements
   - Architecture (with tradeoff options)
   - Tech Stack (opinionated recommendations)
   - Design & Security
4. Generate SPEC.md + CLAUDE.md
5. Generate SPEC/ supplements if user agreed
6. Offer compound engineering session prompt (`prompt.md`)

### Allowed Tools
- AskUserQuestion, Write, Read, Glob, Grep
- TodoWrite, Task
- mcp__plugin_context7_context7__resolve-library-id
- mcp__plugin_context7_context7__query-docs

---

## Plugin: suno-composer (v5.5.0)

### Purpose
Compose songs optimized for Suno AI music generation with adaptive preference learning and dual-mode workflows. Supports multiple genres, artist references, album composition, variations, and song continuations.

### Core Principle
**Songwriter first, rule-follower second.** References are creative palette, not prescription.

### Commands

| Command | Description | Mode |
|---------|-------------|------|
| `/suno` | Main composition workflow | Auto-detect |
| `/suno [theme]` | Start with creative direction | Vision-First |
| `/suno like <artist>` | Use artist profile | Vision-First |
| `/suno <tier>` | Use J-pop tier preset | Vision-First |
| `/suno album about [concept]` | Multi-track album/EP | Album (auto-detected) |
| `/suno acoustic version of [song]` | Transform existing song | Variation (auto-detected) |
| `/suno sequel to [song]` | Create song continuation | Extend (auto-detected) |

### Key Features
- **Dual-mode system**: Vision-first (Claude proposes) and guided (step-by-step wizard), auto-detected from input
- **Conversational intent detection**: Album, variation, extend modes detected from natural language
- **Adaptive preferences**: First-run wizard (2-3 questions) + session reflection
- **Narrative style prompts**: Arrangement as temporal journey, not tag lists
- **29 artist profiles**: YOASOBI, Ado, Aimer, Eve, and more
- **5 J-pop tier presets**: anisong, surface, mainstream, doujin, legacy
- **Sparse tagging**: Only 3-4 technique tags at inflection points
- **Top-anchor strategy**: Lead style prompts with vocal persona
- **File output**: Organized markdown files in `./songs/` directory

### Components

```
plugins/suno-composer/
├── .claude-plugin/
│   └── plugin.json              # Plugin manifest (v5.5.0)
├── commands/
│   └── suno.md                  # Main composition command
├── skills/
│   └── suno/
│       ├── SKILL.md             # Skill definition (single source of truth)
│       └── references/          # 15 reference files
│           ├── artist-profiles.md
│           ├── jpop-tiers.md
│           ├── suno-metatags.md
│           ├── pro-techniques.md
│           ├── genre-deep-dive.md
│           ├── japanese-lyric-patterns.md
│           ├── album-composition.md
│           ├── variation-patterns.md
│           ├── continuation-patterns.md
│           ├── output-formats.md
│           ├── song-structures.md
│           ├── user-preferences.md
│           ├── workflow-modes.md
│           ├── walkthroughs.md
│           └── troubleshooting.md
├── examples/
│   └── suno-composer.local.md   # User preferences template
├── README.md
├── SPEC.md                      # Feature spec for v5.3-5.4
├── SPEC-5.4.1.md                # Feature spec for walkthroughs & troubleshooting
├── CLAUDE.md
└── LICENSE
```

### Output Format
Each song includes:
- **Style Prompt**: 8-15 elements in flowing prose for Suno's "Style of Music" field
- **Lyrics**: Structured with section markers and sparse technique tags
- **Specifications**: Tempo, vocal, instruments, production style, inflection points

### Preference Locations
- Global: `~/.claude/suno-composer.local.md`
- Project: `.claude/suno-composer.local.md`
- Merge logic: Project sections override matching global sections, global fills gaps

### Allowed Tools
- Read, Glob, AskUserQuestion, Write, Skill

---

## Plugin: kana-code-rpc (v0.5.0)

### Purpose
Display Claude Code activity as Discord Rich Presence. Shows project name, current tool activity, model, token usage, cost, git branch, lines changed, agent name, and context warnings. Features PID-based session tracking, multi-session daemon, idle detection, and YAML configuration with hot-reload.

### Core Principle
**Statusline as primary data source.** The statusline API feeds token, cost, duration, lines, agent, and context data; hooks handle session lifecycle and tool updates.

### Architecture

```
Claude Code Session                    Terminal
  ├── SessionStart hook ──────┐
  ├── PreToolUse hook ────────┼──→ state.json ←── daemon ──→ Discord IPC
  ├── SessionEnd hook ────────┘        ↑
  └── Statusline API ─── statusline.py ┘──→ Terminal display
```

### Data Flow

| Source | Data | Target |
|--------|------|--------|
| SessionStart hook | PID, project, branch | state.json + sessions.json |
| PreToolUse hook | tool name, filename | state.json |
| Statusline API | model, tokens, cost, duration, lines, agent, context % | state.json (via statusline.py) |
| SessionEnd hook | PID | sessions.json (remove) |
| Daemon (1s poll) | state.json → Discord RPC | Discord IPC |

### Hook Configuration

| Hook | Trigger | Timeout | Async |
|------|---------|---------|-------|
| SessionStart | Session opens | 15s | yes |
| PreToolUse | Before tool execution (15 tool types + MCP) | 10s | yes |
| SessionEnd | Session closes | 10s | yes |

### Discord Display

**Details line** (max 120 chars): `{activity} [{file}] on {project} ({branch})`

**State line** (bullet-separated): `Model • Tokens • Cost • Lines • Context`

| Display Element | Source | Example | Config Toggle |
|----------------|--------|---------|---------------|
| Activity | PreToolUse hook tool name | "Editing main.py" | — |
| Agent awareness | statusline agent.name + Task tool | "Delegating to code-reviewer" | — |
| File | PreToolUse hook tool_input | "[presence.py]" | `show_file` |
| Model | statusline model.display_name | "Opus 4.6" | `show_model` |
| Tokens | statusline context_window tokens | "22.9k tokens" | `show_tokens` |
| Cost | statusline cost.total_cost_usd | "$0.18" | `show_cost` |
| Git branch | statusline workspace.project_dir | "(main)" | `show_branch` |
| Lines changed | statusline cost.total_lines_added/removed | "+156 -23" | `show_lines` |
| Context warning | statusline context_window.used_percentage | "⚠ 85% ctx" / "🔴 97% ctx" | `show_context_warning` |
| Idle | No activity > idle_timeout | "Idling" | `idle_timeout` |
| Token cycling | 8s period (5s simple, 3s cached) | input + cache_read | — |

### Session Management

- Sessions tracked by PID (Claude Code's ancestor process ID found via parent chain walking)
- Session liveness via PID alive checks (ctypes `OpenProcess`/`GetExitCodeProcess` on Windows, `os.kill` on Unix)
- Orphan cleanup: dead PIDs auto-removed (checked every 30s)
- Single daemon shared across all sessions (PID file prevents duplicates)

### Configuration (`config.yaml`, hot-reloaded every 30s)

| Setting | Default | Description |
|---------|---------|-------------|
| `discord_app_id` | null | Custom Discord app ID (17-19 digits) |
| `display.show_tokens` | true | Show token count |
| `display.show_cost` | true | Show API cost |
| `display.show_model` | true | Show model name |
| `display.show_branch` | true | Show git branch |
| `display.show_file` | true | Show filename when editing |
| `display.show_lines` | true | Show lines added/removed |
| `display.show_context_warning` | true | Show context warning at >80% |
| `idle_timeout` | 300 | Seconds before "Idling" (1-86400) |

### Components

```
plugins/kana-code-rpc/
├── .claude-plugin/
│   ├── plugin.json              # Plugin manifest (v0.5.0)
│   └── config.yaml              # User configuration (hot-reloaded)
├── hooks/
│   └── hooks.json               # SessionStart, PreToolUse, SessionEnd
├── scripts/
│   ├── presence.py              # Main entry + session management (~1,300 lines)
│   ├── state.py                 # File-locked state (~290 lines)
│   └── statusline.py            # Terminal display + state feed (~235 lines)
├── README.md
├── SPEC.md
├── CLAUDE.md
└── LICENSE
```

### Cross-Platform Support
- **File locking**: msvcrt.locking (Windows) / fcntl.flock (Unix)
- **Daemon spawn**: subprocess.Popen + CREATE_NO_WINDOW (Windows) / fork + setsid (Unix)
- **Daemon kill**: taskkill (Windows) / SIGTERM (Unix)
- **Data directory**: %APPDATA%/kana-code-rpc (Windows) / ~/.local/share/kana-code-rpc (Unix)
- **Atomic writes**: tempfile.mkstemp + os.replace (both platforms)

### Reliability
- Connection retry: 12 attempts (1 min) before giving up
- Circuit breaker: exits after 10 consecutive errors
- Graceful PyYAML fallback (uses defaults if not installed)
- Atomic writes prevent state corruption
- Lock timeout logging to stderr + lock_error.log

---

## Technical Architecture

### Repository Structure

```
kana-code-plugins/
├── .claude/
│   └── settings.local.json      # Local Claude settings
├── .claude-plugin/              # Root plugin config
│   └── marketplace.json         # Marketplace manifest
├── plugins/
│   ├── project-spec/            # Specification generator plugin
│   ├── suno-composer/           # Song composition plugin
│   └── kana-code-rpc/           # Discord Rich Presence plugin
├── .gitignore
├── LICENSE                      # MIT
├── README.md                    # Marketplace documentation
├── SPEC.md                      # This file
└── CLAUDE.md                    # Agent-optimized pointer file
```

### Plugin System Architecture
Claude Code plugins use a modular structure:
- **Commands** (`commands/*.md`) — Slash commands users invoke directly
- **Skills** (`skills/*/SKILL.md`) — Domain knowledge loaded on demand via trigger patterns
- **Agents** (`agents/*.md`) — Autonomous subagents for complex tasks
- **Hooks** (`hooks/hooks.json`) — Event-driven automation (SessionStart, PreToolUse, SessionEnd, etc.)
- **References** (`skills/*/references/`) — Lookup material for skills
- **plugin.json** — Plugin manifest with metadata, version, keywords

### Installation

```bash
# Add marketplace
/plugin marketplace add Linaqruf/kana-code-plugins

# Install individual plugins
/plugin install project-spec@kana-code-plugins
/plugin install suno-composer@kana-code-plugins
/plugin install kana-code-rpc@kana-code-plugins

# Or run directly during development
claude --plugin-dir /path/to/kana-code-plugins/plugins/project-spec
```

### Dependencies
- **Context7 MCP**: Used by project-spec for fetching tech documentation
- **pypresence**: Used by kana-code-rpc for Discord IPC
- **pyyaml** (optional): Used by kana-code-rpc for config parsing

### Plugin Manifest Schema
```typescript
interface PluginManifest {
  name: string;
  version: string;
  description: string;
  author: { name: string };
  keywords: string[];
  license?: string;
  repository?: string;
}
```

---

## File Organization Patterns

### Command File Structure
Commands use YAML frontmatter (name and version derived from filename and plugin.json):
```yaml
---
description: What this command does
argument-hint: "[optional args]"
allowed-tools:
  - ToolName
---

# Command Body (Markdown)
Instructions for Claude when command is invoked
```

### Skill File Structure
```yaml
---
name: skill-name
description: Trigger patterns and use cases
version: x.x.x
---

# Skill Body (Markdown)
Knowledge, conventions, reference pointers
```

### Agent File Structure
```yaml
---
name: agent-name
description: Agent purpose
version: x.x.x
model: sonnet
color: blue
whenToUse: |
  Examples of when to spawn this agent
tools:
  - Read
  - Write
---

# Agent Instructions (Markdown)
```

---

## Design Decisions

### Why SPEC.md + SPEC/ Pattern
- Single source of truth (SPEC.md) that's always complete
- Supplements only for reference material (schemas, SDK patterns)
- Trigger-based references prevent context pollution

### Why Dual-Mode in Suno Composer
- Vision-first for experienced users who know what they want
- Guided mode for exploration and discovery
- Auto-detection based on input richness reduces friction

### Why Narrative Style Prompts (v5.4+)
- Suno v5 interprets prose as arrangement instructions
- Tag lists buried the genre under production descriptors
- Narrative flow naturally incorporates temporal progression

### Why PID over session_id (kana-code-rpc v0.5.0+)
- PID-based tracking is deterministic — no dependency on statusline API delivering session_id
- Hooks run in subprocess context where parent chain walking reliably identifies the Claude Code process
- PID alive checks (ctypes on Windows, os.kill on Unix) are instantaneous vs timestamp staleness requiring periodic touching

### Why Statusline as Primary Data Source
- Statusline runs every ~300ms with all session data (model, tokens, cost, duration, lines, agent, context)
- Eliminates need for hooks to duplicate data extraction
- Single pipeline: statusline.py → state.json ← daemon → Discord

---

## Development Status

### project-spec v4.0.0
- [x] Unified `/spec-writing` command with argument-based routing
- [x] Existing codebase detection and document/plan/both modes
- [x] Feature gap analysis
- [x] Design audit and overhaul flow
- [x] Context7 integration
- [x] Compound engineering session prompts (`prompt.md`)
- [x] Template library (11 section templates)
- [x] Example specifications (7 examples)
- [x] Opus 4.6 adaptive thinking optimization
- [x] Progressive disclosure (SKILL.md + references/)

### suno-composer v5.5.0
- [x] Vision-first mode with conversational intent detection
- [x] Guided mode with step-by-step wizard
- [x] Artist profiles (29 artists)
- [x] J-pop tier system (5 tiers)
- [x] Album, variation, and extend modes (auto-detected)
- [x] Narrative style prompts (v5.4+)
- [x] Adaptive preferences: first-run wizard + session reflection (v5.3+)
- [x] Opus 4.6 prompt optimization (v5.5.0)
- [x] 15 reference files
- [x] Walkthroughs and troubleshooting guides (v5.4.1+)

### kana-code-rpc v0.5.0
- [x] Discord Rich Presence with multi-session daemon
- [x] Session lifecycle hooks (SessionStart, PreToolUse, SessionEnd)
- [x] Terminal statusline display (Apple Finder breadcrumb style)
- [x] Token/cost tracking with cache-aware cycling display
- [x] PID-based session tracking (ctypes on Windows, os.kill on Unix)
- [x] Statusline API integration (duration, lines, agent, context)
- [x] Agent awareness ("Delegating to {agent_name}")
- [x] Lines changed display (+N -N)
- [x] Context warnings (⚠ >80%, 🔴 >95%)
- [x] YAML config with hot-reload and validation
- [x] Cross-platform support (Windows + Unix)
- [x] Connection retry, circuit breaker, atomic writes
- [x] PID file race protection, log rotation, multi-session clobbering fix

---

## Open Questions

- [ ] Should there be a shared utility layer between plugins?
- [ ] Add more genre coverage to suno-composer (genres beyond J-pop)?
- [ ] Consider plugin testing framework?
- [ ] Explore preference sync across machines (currently out of scope)
- [ ] Should kana-code-rpc support Windows named pipes for faster IPC?

---

## References

### External
- [Claude Code Plugin Documentation](https://docs.anthropic.com/claude/code/plugins)
- [Claude Code Statusline API](https://code.claude.com/docs/en/statusline)
- [Suno AI](https://suno.com) — Music generation platform
- [Context7 MCP](https://context7.dev) — Documentation fetching

---

*Generated with project-spec plugin for Claude Code*
