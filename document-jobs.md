# Document Jobs — Detailed Specifications

Each document in the setting system has ONE job. This file defines what
belongs in each document, what doesn't, ∧ includes structural examples
showing what finished documents look like in practice.

See `SKILL.md` Key Terms section for definitions of compaction, corpus bias,
behavior-scoping, PC, NPC, and other terms used throughout this document.

---

## Operations Manual (in the runtime skill file)

**Finalized LAST.** This is the runtime registry — it tells the drafting AI
instance what files exist (the bootstrap load list), when to consult them
(the re-read map), and their priority (the stack, echoed in the setting's
CLAUDE.md). If a document isn't in the bootstrap list, the AI doesn't know
it exists.

**Legacy note:** older settings carried this job in a standalone
`Project_Instructions_[Setting].md`. A PI file found on disk marks a
pre-skill-file build — fold its contents into the runtime skill file when
that setting is next refined; build a new one only if a target platform
requires it.

**Contains:**
- Complete document registry with filenames (the bootstrap load list)
- Session protocol (bootstrap hard gate ∧ canary, what to read at start,
  Distill/Journal cadence — Distill auto-loads, Journals open on pointer only)
- Document priority stack (which doc wins on conflict — see below)
- Compaction recovery instructions (what to do when context gets heavy and
  rules start getting summarized away; typically: re-read the narrative rules and My_Character)
- Pacing rules (how fast scenes move, what gets expanded vs. compressed)
- Pointers to reference documents and when to consult them (the Re-Reads map)

**Does NOT contain:**
- Story content
- Character voice rules
- World lore
- Anti-patterns (those live in the narrative rules — the other job in the
  same skill file — or in character-specific anti-pattern sections of
  My_Character)

**Document priority stack example:**
```
Priority 1: Narrative rules (in the skill file — overrides all)
Priority 2: My_Character (PC voice, cognition, capabilities — overrides world)
Priority 3: Main_Instructions (world rules, factions, physics)
Priority 3: NPC_Reference (NPC roster)
Reference (on-demand): lore files, supplemental mechanics
```
When two documents conflict, the higher-priority document wins. This stack
lives in the runtime skill file ∧ is echoed in the setting's CLAUDE.md
(Document Priority section) so the runtime instance can resolve conflicts
without guessing — twice, for compaction resilience.

**Structural example (the operations sections of a runtime skill file —
the Narrative Rules share the same file):**
```markdown
# [Setting] — Campaign Runner

## Bootstrap
Hard gate: no fiction before bootstrap completes.
Load all core files:
- [setting path]\CLAUDE.md
- [setting path]\My_Character.md
- [setting path]\NPC_Reference.md
Then glob Compaction_*.md ∧ Distill_*.md in the setting directory; read all
found; report one flat state line. Do not auto-load Journal_*.md.
Canary: output `✅ Bootstrap complete.` + the state line. Stop and wait.

## Authority
Skill first. Core docs next. Recall last.
[DEBUG protocol; OOC authentication gate]

## Turn Contract / Per-Turn Workflow
[the Narrative Rules job — see that section]

## Re-Reads
[per-doc map: which sections to re-read when a need fires; pacing notes]
```

**File:** the runtime skill file (shared with the Narrative Rules), with the
priority stack echoed in the setting's CLAUDE.md.

---

## Narrative Rules (in the runtime skill file)

**Priority 1 (highest). Largely universal across settings. Refined as failure modes are found.**

The single most load-bearing set of rules. Contains how to WRITE the output —
tone, NPC behavior, dialogue rules, formatting, and critically, the
accumulated anti-patterns and behavior-scoping rules that prevent the AI's
default failure modes.

**Contains:**
- Prose quality standards (paragraph-level requirements for forward motion)
- NPC behavior rules (autonomous agents with their own agendas, not response
  functions orbiting the PC)
- Scene construction rules (scenes are webs of competing forces, not
  playlists of beats to check off; forces stay unresolved and interfere)
- Positive structures — the executable thing to BUILD, each making a failure impossible (→ Author the ✅, not the ❌). The universal failures they guard include: The Mannequin, The Closed Loop, Beautiful
  Nothing, Terminal Velocity, The Gap Is The Scene, Thread Compounding, NPC
  Gravity, The Exposition Mouth, The Aftermath Camera, Ambient Knowledge,
  The Empathy Cheat. See `failure-modes.md` in this directory for the full
  catalog with root causes and fixes.
- Behavior-scoping against the Claude Tax (rules that name the specific
  softening the AI will reach for and describe the honest version instead)
- Dialogue rules (back-and-forth required; no monologues that exhaust scene
  meaning; no character delivers complete emotional thesis in one speech)
- Formatting requirements (output length, section structure, etc.)

**Does NOT contain:**
- Setting-specific lore
- Character voice (that's My_Character's job)
- Session protocol (that's the Operations Manual — the other job in the same skill file)

**Structural example (abbreviated — a real narrative-rules set may be 20+
pages). For the full catalog of named anti-patterns with root causes and
fixes, see `failure-modes.md` in this directory. For how to construct new
anti-patterns, see `anti-pattern-methodology.md`:**
```markdown
# Narrative Rules

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

## Behavior-Scoping (against the Claude Tax)
The AI will want to lighten dark content, resolve conflict early, and steer
toward positive outcomes. Name each specific tendency and describe what the
honest version looks like instead.
[Specific rules naming specific tendencies + the correct behavior]
```

**Why most of it carries forward:** Every rule that "looks setting-specific"
is usually compensating for an AI failure mode that persists across settings,
so the proven set ports as a baseline rather than being rebuilt each time.
Refine it deliberately — dropping a load-bearing rule lets a solved failure
return, often undiscovered until mid-campaign when a scene dies to a problem
fixed three iterations ago. Change with intent, not by reflexively "adapting."

**File:** the runtime skill file, alongside the Operations Manual (not a separate document).

---

## Character (My_Character)

**Priority 2. Built FIRST among setting-specific documents.**

The PC: voice, cognitive mode, capabilities, identity. Voice is the core of
this document because everything else flows from getting it right — if the
voice is wrong, perfect world-building doesn't matter. Backstory and
capabilities live here too because they're inseparable from who the character
IS at play-start.

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
- PC abilities, capabilities, limitations (what they can do, what they can't,
  what costs them to use their powers)
- PC identity and current-frame backstory (who they are at play-start — deep
  historical facts live in Character_Lore instead)

**Does NOT contain:**
- World rules / factions / physics (that's Main_Instructions)
- NPC rosters (that's NPC_Reference)
- Pre-conversion / deep historical facts (that's Character_Lore)
- NPC voice (that's the narrative rules)
- Setting-wide mechanics detail (that's Supplemental_Lore)

**Structural example:**
```markdown
# My Character — [Setting Name]

## Primary Cognitive Mode: Architectural
[Character] processes the world as structures. Relationships are
load-bearing or decorative. Conversations have foundations and facades.
Emotions are rooms she enters or avoids. Danger is structural failure.

## Metaphor Framework
Draw from: engineering, construction, spatial reasoning, structural
analysis. Do NOT draw from: nature imagery, musical metaphor, military
metaphor.

## Sentence Patterns
Default: medium-length declarative sentences with occasional sharp
fragments for emphasis. Under stress, clipped. When comfortable, longer.

## Dialogue Voice
Direct, slightly clipped, deflection humor. Avoids emotional vocabulary.
When pressed emotionally, goes monosyllabic or changes subject.

## Capabilities
[What they can do. What costs them. Hard limits.]

## Identity + Current Frame
[Who they are at play-start — NOT deep history.]

## Anti-Patterns

### ❌ The Poet
The AI will want to make her lyrical and emotionally articulate. She is
neither. Precise and evasive. Poem-sounding prose = wrong.

### ❌ The Therapist
She doesn't name emotions and process them aloud. She builds walls and
changes subject. Insight arrives through action.
```

**Key principle:** Voice is built first because everything else flows from
getting it right. Keep to the 150-line cap — details that can live in
Character_Lore (deep history) or Supplemental_Lore (mechanics) should.

**File:** `My_Character_[Setting].md`.

---

## World (Main_Instructions)

**Priority 3. Built AROUND the character.**

The world at play-start. Static reference for how the setting works.

**Contains:**
- World rules and cosmology (magic systems, power structures, physical laws
  that differ from reality)
- Faction/organization structures (who the power players are, what they want,
  where they conflict)
- Geography and setting details
- World-level pre-play context (what state is the world in)

**Does NOT contain:**
- Runtime state
- Prose style rules (that's the narrative rules)
- Character voice/cognitive mode (that's My_Character)
- NPC rosters (that's NPC_Reference)
- Specialized mechanics detail (that's Supplemental_Lore)
- Session protocol (that's the Operations Manual, in the runtime skill file)

**Key principle:** This document describes the world at play-start. It does
NOT accumulate runtime state. State claims in this document are permanent —
every one is a potential trojan. Write "the PC's default mode is isolation"
(describes tendency that can change) not "the PC is isolated" (describes
current state that the AI will refuse to let change). See ❌ State-as-Static
Trojans in `failure-modes.md` for the full failure mode.

**Structural example:**
```markdown
# Main Instructions — [Setting Name]

## World Rules
[How magic/technology/power works. What's possible, what isn't.]

## Factions
[Who the power players are. What each faction wants. Where they conflict.]

## Geography
[Key locations and what makes them significant]

## World Frame at Play-Start
[State of the world as play begins — describe tendencies, not fixed states.
See ❌ State-as-Static Trojans in failure-modes.md]
```

**Key principle:** 100-line cap. Detail that doesn't have to live in the
always-loaded frame belongs in Supplemental_Lore instead. See ❌ Chassis
Inflation in failure-modes.md.

**File:** `Main_Instructions_[Setting].md`.

---

## NPC Reference (NPC_Reference)

**Priority 3. Always loaded.**

Significant recurring NPCs with driver/capability fields. Split from the
character+world docs because the roster tends to grow past what MC/MI's
caps (150/100) can absorb.

**Contains:**

### [NPC Name] ([Character]/[Source media]-coded; secondaries as needed)
- **Role:** [their function in the world independent of the PC]
- **Personality:** [2-4 non-conflicting traits; feeds the runtime's trait-matching checks]
- **DRIVER:** [one sentence — what they spend their life doing, what motivates them across scenes]
- **CAPABILITY:** [one sentence — tool/leverage the driver implies (badge, claim, reach, knowledge, relationship-leverage — open set); what gets EXERCISED when the driver creates conflict with the PC]
- **Relationship to PC:** [the dynamic at play-start]
- **Appearance:** [physical description — face, hair, build, skin, eyes, distinguishing features. Enough to describe them walking into a room.]
- **Outfit:** [what they're wearing — enough to dress them in prose. For settings w/ identity markers (collars, brands, piercings, tattoos, sigils — open set), include material, stone/gem, location on body. These are load-bearing identity details ≠ decorative.]
- **Equipment:** [signature items, weapons, wand/focus form, mount (species + appearance + distinguishing features). Enough to put something in their hand or render something beside them. For characters w/ transformation/warlock/henshin states, include what they look like in that state.]

[Repeat for each significant NPC]

**CRITICAL — render details are load-bearing.** Structural data (drivers,
relationships, faction positions) survives compaction naturally — the runtime
reconstructs WHO and WHY from context. Physical details (appearance, outfit,
equipment) vanish without documentation and can't be reconstructed. An NPC
entry that has a driver and no outfit is a skeleton — the runtime knows what
the character wants but has to invent what they look like every scene,
inconsistently. See ❌ The Skeleton Error in `failure-modes.md`.

**CRITICAL — every character carries a casting reference in its entry header:
character name + source media, BOTH.** (`Makima/Chainsaw Man-coded`,
`HAL 9000/2001 + HK-47/KOTOR coded` — open set; mark secondaries where a
second source helps: `Toby/Quest for Glory IV-coded; Ludo/Labyrinth
secondary`.) The pair routes voice, cadence, body, ∧ want to ONE specific
rendition — the same logic as style anchors, where the work routes truer than
the author: the name picks the character, the source pins WHICH one. Half a
pair decays. Source-only is a vibe the runtime fills from corpus average;
name-only goes invisible the moment context drifts — it reads as just a name
∧ the casting silently stops routing (live case: Deadlight Inn's Toby ∧
Katrina, named FOR Quest for Glory IV's, lost their citations ∧ played
uncoded — the names survived, the casting vanished). Runtime-minted
characters get their pair on first appearance, same as rostered cast (put
the requirement in the minting rules, not just the roster). Codings give
register ∧ driver; they do not import source plot.

**Repairing a half-pair is a lookup, never a guess.** Verify the work exists
∧ pull the missing half FROM the source. Titles collide — when they do, the
citation carries the author (three published novels are named *The Killing
Kind*; the pair reads `Rob Scott/The Killing Kind, Bryan Smith`). A missing
character name is found by matching FUNCTION to the entry (the everyman swept
along by the killer he's bound to → that book's Rob Scott). A character with
several renditions gets WHICH pinned, ∧ the entry's own body votes (a
warm-voiced therapy practice, jokes landing a half-second wrong →
`Hannibal Lecter/Hannibal NBC`, not Hopkins). (Open set.) Every half the
builder supplies is the user's casting to veto — flag it out loud, never land
it silent.

**DRIVER vs CAPABILITY — why both.** Driver is WHAT the NPC pursues (across scenes, not just this one). Capability is HOW the driver gets pressed against the PC when they collide. "Reclaim the escaped asset" is driver; "contract claim, military cyber, enforcer network" is capability. The runtime's per-turn NR lookup pulls both fields directly — no prose extraction, no inference. Recognition of the driver alone isn't enough; the capability tells the runtime what gets EXERCISED against the PC when the driver creates tension. Without an explicit capability field, runtimes default to soft contest (acknowledgment, concern, careful words) when the documented NPC should be pressing hard.

**Generate-as-needed worlds.** For settings that include worlds where NPCs are generated on-the-fly (canon fandoms — The Boys, Hazbin Hotel, Mad Max; or original worlds where populations aren't pre-rostered), provide category-level DRIVER/CAPABILITY templates per typical role. "Supes: DRIVER = narrative control, public image, brand. CAPABILITY = Compound V powers, Vought PR machinery, fame-as-weapon." The templates give invented-on-the-fly NPCs documented driver/capability shapes on first appearance instead of runtime invention from training corpus — which is the worst case for Friction Abdication (made-up NPCs with no documented driver have nothing to hold against evasion).

**Upgrades the older single-field "Agenda"** — split cleanly into Driver (the what) + Capability (the how-it-presses). Settings with existing Agenda entries can be refined in place; the split is a clarification, not a rewrite.

**File:** `NPC_Reference_[Setting].md`.

---

## Lore / Supplemental Documents (On-Demand)

**Reference priority. NOT always loaded — consulted when scene calls for them.**

Static references too large or specialized to fit in the always-loaded frame.
Typical split:

**[Character]_Lore** — Deep pre-play historical facts about the PC. Anything
that isn't needed to draft a typical scene but matters when the PC's history
comes up. Sectioned by topic so the runtime can pull only the relevant slice.

**Supplemental_Lore** — Specialized mechanics: magic system cost tables, tier
breakdowns, faction politics too detailed for MI, time-travel rules, etc.
Sectioned so the runtime pulls only what the current scene invokes.

**Additional roster docs** — Unit references, performer rosters, faction
breakdowns — anything specialized enough that the main NPC_Reference shouldn't
carry it.

**CRITICAL:** Reference documents MUST be registered in the runtime skill
file — in the bootstrap list (always-loaded) ∨ the Re-Reads map (on-demand).
If the skill file doesn't list it, the AI doesn't know it exists.

**File:** standalone files in the setting directory.
