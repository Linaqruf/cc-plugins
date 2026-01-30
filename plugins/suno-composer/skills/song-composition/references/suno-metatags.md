# Suno v5 Metatags Reference

Complete reference for Suno AI v5 metatags, structure tags, vocal styles, and production techniques.

## Structure Tags

Use these section labels in the lyrics field to control arrangement:

| Tag | Purpose | Example |
|-----|---------|---------|
| `[Intro]` | Opening section | `[Intro: Acoustic guitar]` |
| `[Verse]` | Narrative section | `[Verse 1]`, `[Verse 2]` |
| `[Pre-Chorus]` | Build tension | `[Pre-Chorus]` |
| `[Chorus]` | Main hook | `[Chorus]` |
| `[Bridge]` | Contrast/pivot | `[Bridge]` |
| `[Hook]` | Memorable phrase | `[Hook]` |
| `[Break]` | Pause/change | `[Break]` |
| `[Instrumental]` | No vocals | `[Instrumental Break]` |
| `[Outro]` | Closing | `[Outro: Fade out]` |
| `[Build]` | Rising intensity | `[Build]` |
| `[Drop]` | EDM climax | `[Drop]` |
| `[Interlude]` | Musical passage | `[Interlude]` |
| `[Refrain]` | Repeated phrase | `[Refrain]` |

### Structure Best Practices

- Place most important tags at section beginnings
- Use numbered verses: `[Verse 1]`, `[Verse 2]`
- Combine with instruments: `[Intro: Piano]`, `[Outro: Fade out]`
- Clear order: `[Intro] → [Verse 1] → [Pre-Chorus] → [Chorus] → [Verse 2] → [Chorus] → [Bridge] → [Chorus x2]`

## Vocal Style Tags

### Female Vocals

| Style | Tags | Use Case |
|-------|------|----------|
| Soft/Gentle | `soft female vocals`, `breathy`, `intimate`, `gentle voice` | Ballads, intimate songs |
| Powerful | `female belting`, `powerful vocals`, `strong voice`, `emotional delivery` | Anthems, climactic moments |
| Cute/Idol | `cute vocals`, `bright voice`, `idol style`, `youthful` | J-pop, K-pop, upbeat songs |

### Male Vocals

| Style | Tags | Use Case |
|-------|------|----------|
| Soft/Gentle | `soft male vocals`, `tender`, `warm voice`, `gentle delivery` | Ballads, acoustic |
| Powerful | `male belting`, `rock vocals`, `powerful`, `passionate` | Rock, anthems |
| Low/Smooth | `deep voice`, `smooth baritone`, `rich vocals` | R&B, jazz |

### Special Vocal Styles

| Style | Tags | Use Case |
|-------|------|----------|
| Duet | `male and female duet`, `harmonies`, `call and response` | Romantic songs |
| Choir | `choir vocals`, `layered harmonies`, `group vocals` | Gospel, anthems |
| Synth/Vocaloid | `vocaloid style`, `synthesized vocals`, `electronic voice` | Doujin, electronic |
| Whisper | `whisper`, `intimate whisper`, `ASMR-like` | Intros, bridges |
| Raspy | `raspy vocals`, `gritty voice`, `raw` | Rock, blues |
| Melismatic | `melismatic`, `vocal runs`, `R&B style` | Soul, R&B |
| Monotone | `monotone`, `spoken word`, `talk-singing` | Indie, rap |

## Production Tags

### Reverb & Space

| Tag | Effect |
|-----|--------|
| `dry vocal` | Minimal reverb, close/intimate |
| `reverb-heavy` | Spacious, atmospheric |
| `wide stereo` | Expansive mix |
| `intimate` | Close mic, small room |
| `cathedral reverb` | Large, epic space |

### Compression & Dynamics

| Tag | Effect |
|-----|--------|
| `compressed` | Punchy, loud, consistent |
| `dynamic` | Natural volume variation |
| `pumping` | Side-chain compression effect |
| `brick wall` | Maximum loudness |

### Production Style

| Tag | Effect |
|-----|--------|
| `lo-fi` | Vintage, warm, imperfect |
| `polished` | Clean, modern, professional |
| `raw` | Unprocessed, live feel |
| `overproduced` | Dense, layered |
| `minimalist` | Sparse, essential elements |

### Mix Elements

| Tag | Effect |
|-----|--------|
| `side-chained bass` | Pumping bass effect |
| `layered synths` | Multiple synth textures |
| `heavy bass` | Prominent low end |
| `crisp drums` | Sharp, defined percussion |
| `808 bass` | Hip-hop/trap bass style |

## Mood & Energy Tags

| Mood | Tags |
|------|------|
| Upbeat | `energetic`, `bright`, `cheerful`, `danceable`, `happy` |
| Melancholic | `sad`, `emotional`, `yearning`, `bittersweet`, `nostalgic` |
| Energetic | `driving`, `intense`, `powerful`, `anthemic`, `explosive` |
| Dreamy | `atmospheric`, `ethereal`, `soft`, `floating`, `hazy` |
| Intense | `dramatic`, `powerful`, `dark`, `cinematic`, `epic` |
| Chill | `relaxed`, `smooth`, `laid-back`, `groovy`, `mellow` |
| Aggressive | `angry`, `fierce`, `raw`, `heavy`, `brutal` |
| Romantic | `loving`, `tender`, `passionate`, `sensual`, `warm` |

## Genre-Specific Tag Combinations

### J-pop Ballad
```
j-pop, emotional ballad, female vocals, soft voice, piano, strings, melancholic, anime soundtrack
```

### Energetic Doujin
```
j-pop, electronic, vocaloid style, fast tempo, synthesizer, driving beat, energetic, anime opening
```

### City Pop
```
city pop, 80s, funky bass, saxophone, groovy, nostalgic, japanese, smooth vocals
```

### EDM Drop
```
edm, progressive house, build, drop, synth lead, side-chained bass, euphoric, festival
```

### Lo-fi Hip-hop
```
lo-fi hip hop, chill, jazzy samples, vinyl crackle, relaxed, study music, mellow
```

### Rock Anthem
```
rock, powerful vocals, electric guitar, driving drums, anthem, stadium, epic chorus
```

## Advanced Techniques

### Vocal Technique Tags (Recommended)

Describe *how* to sing - use freely for expressive variety:
```
[Verse 1][soft, breathy]
[Verse 2][intimate, tender]
[Bridge][whisper]
[Chorus][belting, powerful]
```

### Instrument/Arrangement Specification (Recommended)

Control instrumentation and texture:
```
[Chorus: Full band with brass section]
[Verse 2: Stripped back, acoustic only]
[Bridge: Piano and voice only]
[Final Chorus: Full arrangement]
```

### Emotion Progression (Use Strategically)

Arrow progressions (`→`) work well for emotional shifts within a section:
```
[Bridge][Mood: vulnerable → hopeful]
[Final Chorus][Mood: bittersweet → triumphant]
```

## Dynamic Control Through Contrast

### The Problem: Monotonous Escalation

When every section stacks escalation words, Suno interprets this as continuous climb:
```
❌ BAD - monotonous escalation:
[Verse 1][building]
[Pre-Chorus][rising tension]
[Chorus][powerful, soaring]
[Verse 2][more intense]
[Bridge][even more powerful]
[Final Chorus][peak, explosive, maximum]
```

### The Solution: Contrast and Variety

Create interest through variety - soft makes loud feel bigger:
```
✅ GOOD - dynamic contrast:
[Intro: Piano, atmospheric]
[Verse 1][soft, intimate]
[Pre-Chorus][building]
[Chorus][full arrangement]
[Verse 2][tender, reflective]
[Bridge][stripped, vulnerable]
[Final Chorus][soaring, triumphant]
[Outro: Fade with piano]
```

### Creating Reset Points

After a peak (Chorus), reset energy to create contrast:
- Verse 2 should return to intimate space (not "bigger than verse 1")
- Bridge is your contrast point - pull back here
- Use `[stripped]`, `[vulnerable]`, `[tender]` to create valleys
- Final Chorus earns its intensity through the preceding contrast

### Tag Categories

| Tag Type | Purpose | Examples |
|----------|---------|----------|
| Vocal technique | How to sing | `[soft, breathy]`, `[belting]`, `[whisper]` |
| Arrangement | Instrumentation | `[piano only]`, `[full band]`, `[stripped]` |
| Emotion | Feeling/mood | `[vulnerable]`, `[triumphant]`, `[tender]` |
| Progression | Within-section shift | `[Mood: lost → found]` |
| Intensity | Climactic peaks | `[soaring]`, `[powerful]` (use at peaks only) |

## Style Prompt vs Lyrics Prompt

**Best Practice**: Separate style description from lyrics.

**Style Prompt** (goes in Style field):
```
dreamy bass house, dreamstep influence, 126 bpm feel, deep sub bass, crisp drums, wide synth pads, airy topline, modern clean mix, festival-ready drop, emotional but restrained
```

**Lyrics Prompt** (goes in Lyrics field):
```
[Verse 1]
Walking through the neon lights...

[Chorus]
We're dancing in the moonlight...
```

## Tag Count Guidelines

- **Minimum**: 6 tags for basic direction
- **Optimal**: 8-12 tags for balanced control
- **Maximum**: 15+ tags may cause conflicts

## Sources

- [How to Prompt Suno](https://howtopromptsuno.com/)
- [Suno Metatag Creator Guide](https://sunometatagcreator.com/metatags-guide)
- [Jack Righteous - Suno Meta Tags Guide](https://jackrighteous.com/en-us/pages/suno-ai-meta-tags-guide)
- [Suno v5 Prompt Patterns](https://plainenglish.io/blog/i-made-10-suno-v5-prompt-patterns-that-never-miss)
