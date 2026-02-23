# kana-code-plugins

A Claude Code plugin marketplace with project specification and song composition plugins.

## Spec Reference

Primary spec: `SPEC.md`

## Plugins

| Plugin | Version | Path | Commands |
|--------|---------|------|----------|
| project-spec | 4.0.0 | `plugins/project-spec/` | `/spec-writing` (project, feature, design, design:overhaul) |
| suno-composer | 5.5.0 | `plugins/suno-composer/` | `/suno` (all modes auto-detected) |
| kana-code-rpc | 0.5.0 | `plugins/kana-code-rpc/` | (hooks only — no commands) |
| anipy-cli | 0.1.0 | `plugins/anipy-cli/` | `/anipy-cli` |

## Key Patterns

### Plugin Structure
```
plugins/<name>/
├── .claude-plugin/plugin.json   # Manifest
├── commands/*.md                # Slash commands
├── skills/<name>/SKILL.md       # Domain knowledge
├── agents/*.md                  # Subagents (optional)
└── README.md
```

### Command Frontmatter
```yaml
---
description: What the command does
argument-hint: "[args]"
allowed-tools: [Read, Write, ...]
---
```

### Skill Frontmatter
```yaml
---
name: skill-name
description: Trigger patterns...
---
```

## Development

```bash
# Test plugin locally
claude --plugin-dir ./plugins/project-spec

# Install from marketplace
/plugin marketplace add Linaqruf/kana-code-plugins
/plugin install project-spec@kana-code-plugins
```

## Key Constraints

- Plugins are self-contained in `plugins/<name>/` directories
- Each plugin must have `.claude-plugin/plugin.json` manifest
- Commands, skills, and agents are discovered by directory convention
- Version bumps require updating both `plugin.json` and `README.md`

## Current Status

→ Check `SPEC.md` for full plugin specifications and development status
