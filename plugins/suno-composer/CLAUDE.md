# Suno Composer

Compose songs for Suno AI with adaptive preference learning.

## Spec Reference

Primary spec: `SPEC.md`

## Key Constraints

- First-run wizard triggers only when NO preferences exist (global or project)
- Preference inheritance: project overrides global, global fills gaps
- Session reflection uses Claude's natural pattern recognition, not mechanical tracking
- Reflection is conversational, not itemized checkboxes

## Commands

| Command | Description |
|---------|-------------|
| `/suno` | Main composition workflow (all modes auto-detected) |

## Current Status

→ Check `SPEC.md` → Development Phases section

**Current version:** v5.5.0 - Opus 4.6 Optimization
