# Changelog

All notable changes to project-spec will be documented in this file.

## [5.0.0] - 2026-02-06

### Breaking Changes
- **Consolidated to single `/spec` command** — All spec types accessible via arguments
  - `/spec feature [name]` replaces `/feature`
  - `/spec design [style]` replaces `/design`
  - `/spec design:overhaul` replaces `/design:overhaul`
  - `/sync` removed — use `/spec` to re-audit project against codebase

### Added
- **Spec type routing** in SKILL.md — Unified type detection and flow dispatch table
- **Gap Analysis section** in SKILL.md — Core algorithm absorbed from deprecated `/feature` command
- **Design Audit section** in SKILL.md — Audit workflow absorbed from deprecated `/design:overhaul` command
- **`references/spec-type-flows.md`** — Detailed workflows for feature, design, and overhaul flows
- **Intent detection** in spec-writer agent — Routes natural language to appropriate spec type

### Changed
- **`/spec` command expanded** to handle project, feature, design, and overhaul via argument parsing
- **spec-writer agent updated** with intent detection table and expanded whenToUse examples
- **SKILL.md restructured** — Added Spec Types, Gap Analysis, Design Audit sections; removed Related Commands

### Removed
- **`/feature` command** — Use `/spec feature` instead
- **`/design` command** — Use `/spec design` instead
- **`/design:overhaul` command** — Use `/spec design:overhaul` instead
- **`/sync` command** — Deprecated; re-audit via `/spec` is more reliable than git-diff inference

## [4.1.0] - 2026-02-06

### Added
- **Smart Batching Rules** — 10-turn interview table with explicit skip conditions per turn
- **Codebase-Aware Skipping** — Auto-detect answers from lockfiles, package.json, config files (8 signals with auto-fill behavior)
- **Auto-Detect Project Type** — Infer CLI/web-app/API/library from codebase signals (7 detection rules)
- **Security section** in output template — Auth flow diagram, input validation checklist, sensitive data protection table
- **Error Handling Strategy section** — Structured error format, error code table, error boundary strategy, retry logic
- **Monitoring & Observability section** — Error tracking, logging levels, health checks, performance monitoring
- **State Diagrams** in System Maps — Entity lifecycle visualization for features with complex status flows
- **Algorithm Specifications** — Explicit rules for non-obvious logic (type inference, search ranking, metadata extraction)
- **Zod validation schemas** alongside TypeScript interfaces in Data Models section

### Changed
- **interview-questions.md** restructured from v3.0 to v4.0 — organized by interview turns with AskUserQuestion format throughout
  - Added conditional follow-up questions (real-time, file storage, payments, email, background jobs)
  - Added quick-start compressed flows for each project type (`/spec web-app`, `/spec cli`, `/spec api`, `/spec library`)
  - Added feature planning and design system question sets
  - Added context-aware adaptations per question
- **output-template.md** enhanced to v4.0 — all new sections with concrete examples
  - Tech Stack table now includes "Alternatives Considered" column with tradeoffs
  - Development Phases include dependency chains between phases
  - Open Questions upgraded to table format with options, impact analysis, and status
  - Writing Guidelines updated with testability and alternatives examples
- **All 5 example specs upgraded** to demonstrate v4.1 patterns:
  - `web-app-spec.md` — Added security section, state diagram (task lifecycle), deep tech rationale with alternatives, testable acceptance criteria, monitoring section
  - `api-spec.md` — Added SPEC/ supplement reference pattern, security section (API key management, SSRF protection, rate limiting), algorithm specs (metadata extraction, search ranking)
  - `feature-spec.md` — Added codebase analysis evidence section, implementation dependency graph, specific edge case tables, validation schemas
  - `cli-spec.md` — Added algorithm specs (type inference rules, validation pipeline), exit code table, security section, error handling strategy
  - `library-spec.md` — Added bundle size strategy with CI enforcement, timezone implementation approach, algorithm specs (format token parsing), subpath export design
- **SKILL.md** enhanced with smarter Context7 query templates per technology category
- **Output quality guidelines** tightened: testable acceptance criteria, Zod validation alongside interfaces, state diagrams for entities with lifecycle

## [4.0.0] - 2026-02-06

### Changed
- **Opus 4.6 optimization** — Restructured all prompts for adaptive thinking and literal interpretation
- **SKILL.md is now the single source of truth** — Commands and agent reference the skill instead of duplicating methodology
  - `commands/spec.md`: 399 → ~100 lines (command-specific logic only)
  - `commands/feature.md`: 331 → ~160 lines (gap analysis + dispatch)
  - `commands/design.md`: 341 → ~100 lines (design interview + dispatch)
  - `commands/design-overhaul.md`: 419 → ~120 lines (audit + dispatch)
  - `agents/spec-writer.md`: 305 → ~80 lines (agent behavior + dispatch)
- **Tightened prompt language** — Imperative mood, explicit conditions, no hedging
  - Removed vague quantifiers ("~15-20 questions", "if applicable")
  - Replaced with clear conditional logic and entry/exit conditions
- **Enhanced codebase analysis** — More file patterns for framework detection and deep scanning
  - Added: middleware, hooks/composables, jobs/workers, types, CI/CD patterns
  - Added: framework detection table (dependency → framework mapping)
- **Updated model IDs** in SDK examples and templates
- **Fixed CLAUDE.md.template** — Removed stale Quick/SPEC/DEEP mode references from v2.0
- `Task` tool added to `/spec` and `/feature` allowed-tools for deeper codebase analysis
- All command versions bumped to v4.0.0 (design-overhaul and sync remained at v2.0.0, later unified to v4.1.0)

### Added
- **Prompt Principles section** in SKILL.md — Documents adaptive thinking markers and literal interpretation rules
- **Constraints section** in SKILL.md — Non-negotiable rules extracted and centralized
- **Framework Detection table** — Maps package.json dependencies to framework identification
- **Enhanced file-to-section mapping** in `/sync` — More patterns for categorizing changes

### Removed
- **Hooks** (`hooks/hooks.json`) — Auto-suggestion and feature planning hooks removed; skill description triggers are sufficient

### Breaking Changes
- Commands no longer contain full interview methodology — they reference SKILL.md
- SKILL.md is the authoritative source for all shared methodology

## [3.1.0] - 2026-01-19

### Added
- **Existing repository detection** in `/spec` command
  - Detects if running on existing codebase (package.json, src/, configs)
  - Asks: "Document existing project", "Plan new project", or "Both"
  - Document mode: scans codebase, extracts tech stack, generates spec from reality
  - Both mode: documents existing + continues with interview for new features
- **Gap analysis** in `/feature` command
  - Compares SPEC.md requirements vs actual codebase
  - Identifies: specced but not implemented, implemented but not specced
  - Suggests features based on patterns (e.g., "You have auth but no 2FA")
  - Presents options for user to choose which feature to spec
- **`/project-spec:design:overhaul` command** - First-principles design redesign
  - Audits current design (styles, components, tokens)
  - Identifies inconsistencies and outdated patterns
  - Fresh interview: "Forget current implementation. What do you want?"
  - Generates new design system with migration checklist
  - Includes deprecation warnings and phase-by-phase update plan

### Changed
- `/spec` version bumped to 3.1.0
- `/feature` version bumped to 3.1.0
- Section numbers updated in commands to accommodate new steps

## [3.0.0] - 2026-01-19

### Changed
- **Single adaptive flow** - Removed Quick/SPEC/DEEP mode selection
  - SPEC.md is always the core, complete specification
  - SPEC/ files are now optional supplements for reference material only
  - Interview offers supplements mid-flow when hitting reference-heavy topics
- **Core principle**: SPEC.md = things you READ, SPEC/ = things you LOOK UP
- **Opinionated recommendations** baked into interview
  - Lead with recommended option + rationale
  - User can always override
  - Examples: bun over npm, Drizzle for ORM, PostgreSQL for database
- **Trigger-based references** in SPEC.md and CLAUDE.md
  - Format: `→ When implementing API endpoints: SPEC/api-reference.md`
- **System Maps section** added to SPEC.md template
  - Architecture diagrams (ASCII)
  - Data model relations
  - User flow diagrams
  - Wireframes

### Added
- **`/project-spec:sync` command** - Git-aware spec drift detection
  - Detects when spec was last updated
  - Analyzes commits since then
  - Maps file changes to spec sections
  - Suggests targeted updates
  - Arguments: `spec`, `design`, `feature`, or none (all)

### Removed
- Quick/SPEC/DEEP mode selection
- `references/interview-questions-deep.md` (consolidated into main file)
- Complex SPEC/ folder structure with numbered files

### Breaking Changes
- No more mode selection at start of `/spec` command
- SPEC/ folder is now optional, not the default for complex projects
- Interview structure simplified (15-20 questions, always grouped)

## [2.0.0] - 2026-01-15

### Added
- **Three output modes**: Quick, SPEC/, and DEEP
  - **Quick**: Single `SPEC.md` file, grouped interview (~15 questions, ~6-8 turns)
  - **SPEC/**: Adaptive folder structure, hybrid interview (~40 questions, ~15 turns)
  - **DEEP**: Full Socratic interview, one question at a time (~50 questions, ~50+ turns)
- **Adaptive SPEC/ folder structure** (UPPERCASE filenames)
  - Foundation files (always): `00-INDEX.md`, `01-OVERVIEW.md`, `02-ARCHITECTURE.md`, `XX-STATUS.md`, `XX-ROADMAP.md`, `XX-CHANGELOG.md`
  - Conditional files (based on project): `XX-FRONTEND.md`, `XX-BACKEND.md`, `XX-DESIGN-SYSTEM.md`, `XX-API-REFERENCE.md`, `XX-CLI-REFERENCE.md`, `XX-DATA-MODELS.md`, `XX-SECURITY.md`, `XX-CONFIGURATION.md`
  - Numbers for ordering only, not fixed positions
- **CLAUDE.md auto-generation** with reflective behavior
  - When to check SPEC/
  - Ask before assuming (use AskUserQuestion)
  - Update specs proactively
  - Self-correction (keep code and spec in sync)
- **Superpowers-inspired interview patterns**
  - Multiple choice inputs via AskUserQuestion options
  - 2-3 alternatives with tradeoffs for key decisions
  - Validation checkpoints (SPEC/ mode: 3 checkpoints, DEEP mode: per-file)
- **Extended interview questions** for SPEC/DEEP modes
  - 8-phase deep interview (Vision, Requirements, Architecture, Frontend, Backend, Security, Design, Process)
  - AskUserQuestion format examples with options
- **15 template files** for SPEC/ folder generation
  - Foundation: index, overview, architecture, status, roadmap, changelog
  - Conditional: frontend, backend, design-system, api-reference, cli-reference, data-models, security, configuration
  - CLAUDE.md template with tech-stack specific commands

### Changed
- **Renamed `project_spec.md` to `SPEC.md`** (more universal, backward compatible with `PROJECT_SPEC.md`)
- **All spec filenames now UPPERCASE** (`SPEC.md`, `FEATURE_SPEC.md`, `DESIGN_SPEC.md`, `SPEC/00-INDEX.md`, etc.)
- `/feature` command now adapts output location
  - If `SPEC/` exists: writes to `SPEC/XX-FEATURE-[NAME].md`
  - Otherwise: writes to `FEATURE_SPEC.md`
- `/design` command now adapts output location
  - If `SPEC/` exists: writes to `SPEC/XX-DESIGN-SYSTEM.md`
  - Otherwise: writes to `DESIGN_SPEC.md`
- spec-writer agent updated to support 3 modes
- SKILL.md updated with dual-mode documentation

### Breaking Changes
- Mode selection now required at start of `/spec` command
- SPEC/ folder structure is adaptive (not fixed 00-17)
- Output filenames changed to UPPERCASE (`SPEC.md` instead of `project_spec.md`)
- Still checks for `PROJECT_SPEC.md` for backward compatibility

## [1.0.3] - 2026-01-09

### Fixed
- Fix hooks.json format: use event-keyed object instead of array wrapper

## [1.0.2] - 2025-01-09

### Added
- `/design` command for design system specifications
  - Style presets: modern, minimal, bold, custom
  - Generates design_spec.md with colors, typography, components, accessibility
- `/feature` command for feature planning
  - Generates feature_spec.md with requirements, technical design, implementation plan
  - Integration with feature-dev skill (code-explorer, code-architect, code-reviewer)
- Design System section in project_spec.md template
  - Visual identity (colors, typography, spacing)
  - Responsive breakpoints
  - Component library selection
  - Accessibility requirements
  - Interaction patterns
- Feature planning hook for `/feature` suggestions
- New examples: library-spec.md, design-spec.md, feature-spec.md
- Error handling guidance in commands
- TodoWrite tool for tracking interview progress

### Changed
- Hook regex improved to reduce false positives (requires sentence-start patterns)
- Skill description trimmed for better display
- Updated keywords in plugin.json for discoverability

### Fixed
- Regex escaping in hooks.json (`\s` → `\\s`)
- Version sync between plugin.json and SKILL.md

## [1.0.1] - 2025-01-08

### Added
- Initial release with `/spec` command
- spec-writer agent for autonomous planning
- Interview workflow with 3 phases (Product, Technical, Constraints)
- Context7 integration for tech stack documentation
- Examples: web-app-spec.md, cli-spec.md, api-spec.md
- Auto-suggestion hook for new projects
