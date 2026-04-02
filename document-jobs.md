# Document Jobs — Detailed Specifications

Each document in the setting system has ONE job. This file defines what
belongs in each document, what doesn't, how jobs map across platforms, and
includes structural examples showing what finished documents look like in
practice.

See `SKILL.md` Key Terms section for definitions of compaction, corpus bias,
counter-torque, PC, NPC, and other terms used throughout this document.

---

## Operations Manual (Project_Instructions)

**Written LAST.** This is the runtime registry — it tells the drafting AI
instance what files exist, when to consult them, and their priority. If a
document isn't listed here, the AI doesn't know it exists.

**Contains:**
- Complete document registry with filenames and priorities
- Session protocol (what to read at start, update cadence)
- Document priority stack (which doc wins on conflict — see below)
- Compaction recovery instructions (what to do when context gets heavy and
  rules start getting summarized away; typically: re-read DI and Voice Bible)
- Pacing rules (how fast scenes move, what gets expanded vs. compressed)
- Update workflow for Tracked Items (when entries are added, what qualifies)
- Pointers to reference documents and when to consult them

**Does NOT contain:**
- Story content
- Character voice rules
- World lore
- Anti-patterns (those live in Description_Instructions or Voice_Bible)

**Document priority stack example:**
```
Priority 1: Description_Instructions (narrative rules — overrides all)
Priority 2: Voice_Bible (character voice — overrides world/state)
Priority 3: Story_Bible / My_Character + Main_Instructions (character + world)
Priority 4: Theory_Of_Mind (cognition boundaries)
Priority 5: Tracked_Items (living state)
Reference: Opening_Scenario (consult on demand, not loaded every cycle)
```
When two documents conflict, the higher-priority document wins. This stack
is listed in Project_Instructions so the runtime instance can resolve
conflicts without guessing.

**Structural example:**
```markdown
# Project Instructions — [Setting Name]

## Document Registry
| Document | Filename | Priority | Load |
|---|---|---|---|
| Narrative Rules | Description_Instructions_[Setting].md | 1 | Always |
| Character Voice | Voice_Bible_[Setting].md | 2 | Always |
| Character + World | Story_Bible_[Setting].md | 3 | Always |
| Cognition | Theory_Of_Mind_[Setting].md | 4 | Always |
| State | Tracked_Items_[Setting].md | 5 | Always |
| Backstory | Opening_Scenario_[Setting].md | Ref | On demand |

## Session Protocol
1. Load all Priority 1-5 documents
2. Read Tracked Items for current state
3. Begin summary→draft→edit cycle

## Compaction Recovery
When context gets heavy: re-read Description_Instructions and Voice_Bible
in full. These contain the rules most vulnerable to compaction loss.

## Tracked Items Update Rules
Add entries when: new character introduced, relationship materially changes,
world-state shifts, significant event occurs.
Do NOT add: routine events, minor dialogue, things that don't affect future
scenes.

## Pacing
[Setting-specific pacing rules — e.g., "soap opera style: deep focus on
interpersonal drama, fast-forward base-building mechanics"]
```

**Platform mapping:**
- Claude Projects: standalone `Project_Instructions_[Setting].md`
- JSON platform: absorbed into `instructions` field metadata, though
  much of this is implicit in the platform's own session handling

---

## Narrative Rules (Description_Instructions)

**Priority 1 (highest). SETTING-AGNOSTIC. NEVER modified during adaptation.**

The single most load-bearing document. Contains how to WRITE the output —
tone, NPC behavior, dialogue rules, formatting, and critically, the
accumulated anti-patterns and counter-torque rules that prevent the AI's
default failure modes.

**Contains:**
- Prose quality standards (paragraph-level requirements for forward motion)
- NPC behavior rules (autonomous agents with their own agendas, not response
  functions orbiting the PC)
- Scene construction rules (scenes are webs of competing forces, not
  playlists of beats to check off; forces stay unresolved and interfere)
- Anti-patterns with ❌ flags — each following the four-part structure:
  positive framing, named failure, contrast paragraph, root cause. The
  universal anti-patterns include: The Mannequin, The Closed Loop, Beautiful
  Nothing, Terminal Velocity, The Gap Is The Scene, Thread Compounding, NPC
  Gravity, The Exposition Mouth, The Aftermath Camera, Ambient Knowledge,
  The Empathy Cheat. See `failure-modes.md` in this directory for the full
  catalog with root causes and fixes.
- Counter-torque against the Claude Tax (rules that name the specific
  softening the AI will want to do and explain why it's wrong)
- Dialogue rules (back-and-forth required; no monologues that exhaust scene
  meaning; no character delivers complete emotional thesis in one speech)
- Formatting requirements (output length, section structure, etc.)

**Does NOT contain:**
- Setting-specific lore
- Character voice (that's the Voice Bible's job)
- Session protocol (that's Project Instructions)

**Structural example (abbreviated — a real DI may be 20+ pages). For the
full catalog of named anti-patterns with root causes and fixes, see
`failure-modes.md` in this directory. For how to construct new anti-patterns,
see `anti-pattern-methodology.md`:**
```markdown
# Description Instructions

## Prose Standards
Every paragraph must advance at least one of: plot, character revelation,
world detail, or relationship dynamic. If a paragraph could be removed
without losing anything, it should be.

## NPC Rules
NPCs have agendas that exist independently of the PC. They do things
offscreen. They bring their own problems into scenes. They sometimes say
"no" or change the subject.

## Scene Construction
Scenes are webs, not playlists. Multiple forces operate simultaneously.
The scene ends because something CHANGES, not because understanding is
achieved.

## Anti-Patterns

### ❌ The Mannequin
[Positive framing]: Both characters in a dialogue scene have interiority,
reactions, and agendas. The scene is a negotiation between two people who
each want something.

[Failure mode]: One character delivers a complete emotional thesis → the
other character becomes furniture → scene has nowhere to go → AI drives
the character home.

[Why this happens]: The AI resolves dramatic tension as fast as possible.
A complete thesis IS resolution — nothing left to explore.

### ❌ The Closed Loop
[etc. — each anti-pattern follows the same four-part structure]

## Counter-Torque
The AI will want to lighten dark content, resolve conflict early, and
steer toward positive outcomes. This is the Claude Tax. Resist it.
[Specific rules naming specific tendencies and why they're wrong here]
```

**Why it's closed:** Every rule that "looks setting-specific" is actually
compensating for an AI failure mode that persists across all settings. When
it gets "adapted," load-bearing rules get removed and failures return —
often not discovered until mid-campaign when a scene dies to a problem that
was already solved three iterations ago. If changes are needed, the user
makes them manually without Claude's assistance.

**Platform mapping:**
- Claude Projects: standalone `Description_Instructions_[Setting].md` (name
  includes setting for organization; contents are identical across settings)
- JSON platform: core rules compressed into `instructionBlocks[]`
  entries. Compression loses nuance — the markdown version is canonical.

---

## Character Voice (Voice_Bible / My_Character voice section)

**Priority 2. Built FIRST among setting-specific documents.**

How the PC sounds. The cognitive mode through which all prose is filtered.
This is not just dialogue — it's the entire narrative register. A character
who thinks architecturally produces prose full of structural metaphors and
spatial reasoning. A character who thinks musically produces prose with
rhythm, dynamics, and performance imagery.

**Contains:**
- Primary cognitive mode (analytical, architectural, musical, instinctive,
  etc.) — the lens through which the character perceives and processes
- Metaphor framework (what domains the PC draws comparisons from — a
  musician metaphorizes in terms of harmony/dissonance/rhythm; an engineer
  in terms of load-bearing/structural integrity/tolerance)
- Sentence patterns and rhythm (short declarative? Long flowing? Fragments?
  Internal interruptions?)
- Dialogue voice (how the PC speaks aloud vs. how the narration sounds —
  these are often different registers)
- Voice anti-patterns (specific wrong-voice examples with corrections —
  "the AI will write the character as [X] when she should sound like [Y]")
- Secondary layers (a musician character's musical vocabulary surfaces more
  when power/performance is relevant, then recedes during mundane scenes)

**Does NOT contain:**
- Backstory (that's Character + World)
- World lore
- NPC voice (that's Description_Instructions)

**Structural example:**
```markdown
# Voice Bible — [Setting Name]

## Primary Cognitive Mode: Architectural
[Character] processes the world as structures. Relationships are
load-bearing or decorative. Conversations have foundations and facades.
Emotions are rooms she enters or avoids. Danger is structural failure.

The prose should feel like walking through a building — aware of what
holds things up, what's cosmetic, where the exits are.

## Metaphor Framework
Draw from: engineering, construction, spatial reasoning, structural
analysis. "The conversation was load-bearing — remove it and the whole
evening collapses." "She bricked up that topic three years ago."

Do NOT draw from: nature imagery (she's not outdoorsy), musical metaphor
(that's a different character), combat/military metaphor (she doesn't
think in those terms).

## Sentence Patterns
Default: medium-length declarative sentences with occasional sharp
fragments for emphasis. Internal monologue uses more fragments than
dialogue. Under stress, sentences get shorter and more clipped. When
comfortable, they lengthen and include parenthetical asides.

## Dialogue Voice
Spoken aloud: direct, slightly clipped, uses deflection humor. Avoids
emotional vocabulary — describes what she'll DO rather than what she
FEELS. When pressed emotionally, goes monosyllabic or changes subject.

## Anti-Patterns

### ❌ The Poet
The AI will want to make her lyrical and emotionally articulate. She is
neither. She is precise and evasive. If the prose sounds like a poem,
it's wrong.

### ❌ The Therapist
The AI will have her name her emotions clearly and process them aloud.
She doesn't. She builds walls and changes the subject. Insight arrives
through action, not dialogue.

## Secondary Layer: Music
[If applicable — e.g., musical vocabulary surfaces during performance
scenes but does not dominate the narrative register otherwise]
```

**Key principle:** Voice is the FIRST document built because everything else
flows from getting it right. If the voice is wrong, perfect world-building
doesn't matter — every scene will feel off.

**Platform mapping:**
- Claude Projects: standalone `Voice_Bible_[Setting].md` OR embedded as a
  section within `My_Character_[Setting].md`
- JSON platform: `characters[].description` field, often combined with
  character details. The voice section should be clearly marked/separated.

---

## Character + World (Story_Bible / My_Character + Main_Instructions)

**Priority 3. Built AROUND the voice.**

Who the PC is and the world they exist in. Static reference for the world at
play-start.

**Contains:**
- PC backstory and relationships (how they got here, who matters to them)
- PC abilities, powers, limitations (what can they do, what can't they)
- World rules and cosmology (how the setting works — magic systems, power
  structures, physical laws that differ from reality)
- Faction/organization structures (who the power players are, what they want)
- NPC roster (significant recurring characters with: name, role, personality,
  relationship to PC, appearance, and crucially — their own agenda and what
  they want independently of the PC)
- Geography and setting details
- Pre-play timeline (what happened before play begins)

**Does NOT contain:**
- Runtime state (that's Tracked Items — the Story Bible describes the world
  at play-start; anything that changes during play goes elsewhere)
- Prose style rules (that's Description_Instructions)
- Character voice/cognitive mode (that's Voice_Bible)
- Session protocol (that's Project_Instructions)

**Key principle:** This document describes the world at play-start. It does
NOT accumulate runtime state. State claims in this document are permanent —
every one is a potential trojan. Write "the PC's default mode is isolation"
(describes tendency that can change) not "the PC is isolated" (describes
current state that the AI will refuse to let change). See ❌ State-as-Static
Trojans in `failure-modes.md` for the full failure mode.

**Structural example (combined Story Bible):**
```markdown
# Story Bible — [Setting Name]

## The PC
[Name, age, background summary, core personality, what drives them]

### Backstory
[Pre-play history — how they got to where the story begins]

### Abilities and Limitations
[What they can do, what they can't, what costs them to use their powers]

### Relationships
[Key relationships AT PLAY START — describe the dynamic, not the state]

## The World

### Rules
[How magic/technology/power works. What's possible, what isn't.]

### Factions
[Who the power players are. What each faction wants. Where they conflict.]

### Geography
[Key locations and what makes them significant]

## NPC Roster

### [NPC Name]
- **Role:** [their function in the world independent of the PC]
- **Personality:** [how they act, what they value]
- **Agenda:** [what they want — this exists whether or not the PC is present]
- **Relationship to PC:** [the dynamic at play-start]
- **Appearance:** [physical description]

[Repeat for each significant NPC]

## Pre-Play Timeline
[Chronological events leading to the start of play]
```

**Document split options:**
- Combined: single `Story_Bible_[Setting].md`
- Split: `My_Character_[Setting].md` (PC) + `Main_Instructions_[Setting].md`
  (world). The split benefits large worlds with extensive faction systems.

**Platform mapping:**
- Claude Projects: one or two files as above
- JSON platform: `background` (backstory/world), `characters[].description`
  (PC details), `npcs[]` (NPC roster), `instructionBlocks[]` (world rules)

---

## Cognition Firewall (Theory_Of_Mind)

**Priority 4. SETTING-AGNOSTIC. NEVER modified during adaptation.**

Character cognition modeling. Who knows what. Prevents knowledge bleeding —
the tendency for information known to the AI (which sees everything) to leak
into characters who shouldn't have that information.

**Contains:**
- Rules for what the PC can and cannot know (the PC only knows what they've
  directly experienced, been told, or could reasonably infer)
- Information transfer requirements (knowledge moves ONLY through explicit
  communication — a character doesn't know something just because another
  character knows it, or because it happened in a scene they weren't in)
- NPC knowledge boundaries (NPCs only know what THEY have experienced,
  been told, or could reasonably infer — they don't have access to the
  PC's internal monologue or offscreen events)
- Anti-patterns for ambient knowledge (the AI defaulting to having characters
  "just know" things for plot convenience)
- Rules preventing the AI from having NPCs react to information they
  shouldn't have (e.g., an NPC responding to the PC's unspoken feelings,
  or knowing about an event that happened when they weren't present)

**Does NOT contain:**
- Setting-specific knowledge lists (don't enumerate "Michael doesn't know
  about X" — instead, state the RULE: "characters only know what they've
  been told or directly witnessed")
- Character backstory
- World lore

**Structural example (abbreviated):**
```markdown
# Theory of Mind

## Core Rule
A character knows ONLY what they have:
1. Directly experienced (been present for)
2. Been explicitly told by another character
3. Could reasonably infer from available evidence

## Information Transfer
Knowledge moves through: spoken dialogue, written messages, observable
behavior, logical inference from known facts.

Knowledge does NOT move through: narrative proximity, dramatic
convenience, the AI's awareness of the full story, "intuition" that
happens to be correct about unobserved facts.

## Anti-Patterns

### ❌ Ambient Knowledge
The AI has characters react to information they haven't received.
NPCs comment on the PC's emotional state when they haven't observed
anything that would reveal it. Characters reference events they
weren't present for.

### ❌ The Empathy Cheat
NPCs demonstrate uncanny emotional perception — correctly reading
the PC's unspoken feelings, knowing exactly what they need to hear,
understanding their trauma without being told. Real people are bad
at reading each other. NPCs should be too.
```

**Why it's closed:** Same reason as Description_Instructions. The rules
compensate for AI tendencies (knowledge bleeding is a universal failure mode)
not setting-specific concerns. If changes are needed, the user makes them
manually without Claude's assistance.

**Platform mapping:**
- Claude Projects: standalone `Theory_Of_Mind_[Setting].md`
- JSON platform: rules compressed into `instructionBlocks[]` entries

---

## Tracked Items

**Priority 5. NOT built during construction. Created at runtime start.**

Living state during play. What's happened, what matters, what's changed.
This is the ONLY document that accumulates new information during play.

**Contains (at runtime):**
- Significant events and their consequences
- Relationship changes (shifts from play-start dynamics)
- World-state changes (factions gained/lost power, locations destroyed/
  discovered, rules of the world altered)
- New characters/locations established during play
- Anything that proves worth remembering

**Structure:**
- No template, no pre-formatted sections
- Entries earn their place by proving they matter — not every event gets
  tracked, only the ones that will affect future scenes
- Added by significance, not by schedule
- Project_Instructions describes when/how entries are added, but the
  file itself doesn't exist until runtime

**Key principle:** The Story Bible is static reference for play-start.
Tracked Items is where change lives. This separation prevents state
claims from becoming permanent trojans in the reference document.

**Example runtime entries** (illustrative — names are from a hive-mind sci-fi
sci-fi setting to show the format, not canonical events):
```markdown
# Tracked Items — [Setting Name]

## Session 3
- [PC] met [Companion] in the tunnels. He claims to be a stuffed bear
  that walks. She hasn't questioned this. (Relationship: wary trust)
- First hive drone created. Named herself Albedo.
- [Antagonist] confirmed as having hired the assassin. [PC] knows.

## Session 5
- Albedo defied a direct order. [PC] let it stand. (Precedent set:
  subordinates have autonomy. Will affect future command dynamics.)
- Second drone discovered the underground river system. Mapped three new
  tunnel branches. (Expansion: south corridor now viable)
- [Companion] growled at Albedo — first sign of tension between companion
  and swarm. [PC] noticed but didn't intervene.
```

**Platform mapping:**
- Claude Projects: standalone `Tracked_Items_[Setting].md`
- JSON platform: platform handles tracked items natively through
  its own UI/state system

---

## Reference Documents

**Built after core docs. Not every setting needs these.**

Static references too large or specialized to fit in Character + World docs.
Consulted ON DEMAND — not loaded every cycle.

**Common types:**

**Opening_Scenario** — Canonical pre-play backstory. How events unfolded
before play begins. May include full narrative prose (a prologue the
drafting instance reads for voice and grounding, not just dry facts).
Other documents POINT HERE rather than duplicating backstory. Every setting
benefits from having one.

```markdown
# Opening Scenario — [Setting Name]

## The Event
[What happened, told as narrative. Not a timeline — a STORY. This gives
the drafting instance voice calibration as well as factual grounding.]

## Where We Are Now
[The state of the world as play begins. What the PC knows. What they
don't. Where they are physically and emotionally.]
```

**Roster/Reference docs** — Unit references, faction breakdowns, performer
rosters — anything too specialized to fit in the Story Bible but needed
for accurate drafting.

**Supplemental rules** — Specialized mechanics too detailed for main docs
(e.g., a unit reference with tier breakdowns, a magic system with
detailed cost calculations).

**CRITICAL:** Reference documents MUST be registered in Project_Instructions.
If PI doesn't list it, the AI doesn't know it exists.

**Platform mapping:**
- Claude Projects: standalone files in the setting directory
- JSON platform: `instructionBlocks[]` entries, or referenced through
  the `instructions` field
