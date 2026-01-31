# Narrative Style Prompts Design

**Date:** 2026-01-31
**Plugin:** suno-composer
**Version:** 5.3.2 → 5.4.0

## Problem

Style prompts generated as comma-separated tag lists often produce unbalanced or broken results in Suno v5. The same prompts work correctly when transformed into narrative descriptions (via Suno Enhance or manual rewriting).

## Discovery

Real-world testing revealed a pattern:

| Tag List (broken) | Narrative (works) |
|-------------------|-------------------|
| Static ingredient list | Dynamic arrangement story |
| Comma-separated | Flowing sentences |
| Tags stacked randomly | Tags in temporal context |
| Emotion arc appended at end | Arc woven throughout |
| Genre tags piled (5+) | Single genre anchor |

**Key insight:** Suno v5 interprets narrative flow as arrangement instructions. It "hears" the temporal structure.

## Solution

Update SKILL.md guidance to teach the **narrative principle** rather than a checklist of ingredients. Claude applies it creatively per song (consistent with "songwriter first" philosophy).

### The Narrative Principle

Style prompts work best as **narrative descriptions of how the song unfolds** rather than comma-separated tag lists. Describe the arrangement journey: what opens it, how elements enter, how it builds, how it resolves.

**Narrative elements:**
- **Temporal words**: "opens with", "enters", "builds through", "intensifies", "resolves into"
- **Arrangement story**: How instruments and vocals appear and interact over time
- **Contextual tags**: Place descriptors within sentences, not as standalone lists
- **Single genre anchor**: One clear genre reference instead of stacking similar tags

## Examples

### Example 1: Minangkabau Ballad (月夜の手紙)

**Tag List (unbalanced):**
```
warm female vocals with melismatic ornamentation, Minangkabau pop, Indonesian keyboard pop, orgen tunggal style, slow ballad around 88 bpm, electronic organ melody, synthesizer pads, electric keyboard arpeggios, programmed drums with dangdut influence, electric bass, Melayu pop ballad, West Sumatran pop, deeply melancholic, bittersweet longing, emotion arc: quiet reflection → building longing → emotional confession → resigned acceptance
```

**Narrative (works):**
```
This slow Minangkabau pop ballad at 88 bpm opens with gentle electric keyboard arpeggios and soft synth pads. The electronic organ weaves a flowing melody as warm female vocals use melismatic ornamentation. Programmed drums with dangdut nuances and deep electric bass enter, gradually intensifying through layers of Indonesian keyboard pop textures. The arrangement builds from introspection to an emotionally charged climax before resolving into a sparse, tender outro, embodying bittersweet West Sumatran melancholy.
```

### Example 2: Vocaloid Rock (ループの果て)

**Tag List (distorted, broken):**
```
Vocaloid intense electronic rock, Hatsune Miku style synthesized female vocals with powerful emotional delivery, aggressive synth-rock with dramatic builds, 160 bpm high energy driving tempo, heavy distorted synths layered with orchestral stabs, pounding electronic drums, bass drops, glitch breakdowns representing loop destabilization, emotion arc: furious self-confrontation → desperate struggle → moment of clarity → determined resolve
```

**Narrative (works perfectly):**
```
Blazing at 160 bpm, this Vocaloid intense electronic rock track surges with emotionally charged synthesized female vocals. Aggressive distorted synths and orchestral stabs power the verses, while pounding electronic drums and seismic bass drops intensify dramatic builds. Glitch-heavy breakdowns symbolize destabilizing loops, morphing from fierce sonic assaults to urgent melodic progressions. The energy escalates through furious, then desperate, sections, peaking at a clarity-led instrumental bridge before resolving in a determined final explosion of layered synths and relentless beats.
```

## File Changes

### 1. `skills/song-composition/SKILL.md`

**Section:** "Style Prompt Construction" (~lines 82-114)

Replace the numbered checklist with the narrative principle. Keep top-anchor strategy (vocal first) as part of the narrative guidance.

### 2. `skills/song-composition/references/suno-metatags.md`

**Section:** "Style Prompt vs Lyrics Prompt" (~lines 247-265)

Add the two before/after examples inline to demonstrate the principle.

### 3. Version Bumps

- `.claude-plugin/plugin.json`: 5.3.2 → 5.4.0
- `commands/suno.md` frontmatter: 5.3.2 → 5.4.0
- `README.md`: Update "What's New" section

## Out of Scope

- No changes to `/examples` folder
- No new reference files
- No template system (Claude applies principle creatively)
- No genre-specific variations (single principle covers all)

## Success Criteria

- First-try success rate improves (fewer Suno Enhance needed)
- Style prompts read as arrangement descriptions, not tag lists
- Principle is simple enough to remember and apply
