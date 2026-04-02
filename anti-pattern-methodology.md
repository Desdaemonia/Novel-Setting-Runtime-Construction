# Anti-Pattern Methodology

Anti-patterns are the primary quality control mechanism across all settings.
They are not restrictions. They are stage directions correcting the AI's
default impulses toward safe, consequence-free, boring output. This document
describes how to identify, construct, maintain, and evolve them.

See `SKILL.md` Key Terms section for definitions of corpus bias,
counter-torque, compaction, and other terms used throughout.

---

## The Construction Pattern

Every anti-pattern follows a four-part structure. All four parts are required
because each serves a different function in preventing the failure.

### 1. Install the Positive Framing First

Before naming what's wrong, describe what right looks like. The AI needs a
target to aim for. Without one, it falls back on corpus bias (strong
training-data associations that produce genre-template output) — which is
usually what created the failure in the first place.

The positive framing answers: "What does a GOOD version of this scene/voice/
behavior look like, specifically?"

**Example** (from Frost, a literary fiction setting about a marriage where
the husband wrote an unpublished erotic novel about his wife — intimate
scenes needed to feel mundane and real, not cinematic):
```
What a real scene in this register looks like: mundane logistics, physical
negotiation, small awkwardnesses, genuine uncertainty about whether this is
working, bodies that don't cooperate perfectly, moments of connection that
arrive unexpectedly between the mechanical parts.
```

### 2. Name the Failure Mode with a ❌ Flag

Give it a name. The name is a handle that future instances can grep for in
their own output. If the impulse doesn't have a name, the AI can't recognize
it as the impulse-it-was-warned-about.

The ❌ flag is a visual marker that makes the entry scannable and
distinguishes named failure modes from general instructions.

**Example:**
```
❌ Operatic Fantasy — the AI defaults to: candlelit rooms,
significant eye contact, perfectly choreographed escalation, ceremonial
gravity, and emotional transcendence. This is corpus bias, not reality.
```

### 3. Add a Contrast Paragraph

Show the wrong version and explain WHY it's wrong. Not just "don't do this"
— explain what about the AI's training data or structural impulses produces
this failure and why it undermines the setting.

The contrast paragraph teaches the AI to recognize its own impulse. Without
this, the AI can't distinguish between the failure mode and legitimate uses
of similar elements.

**Example:**
```
The wrong version: Michael leads Natasha into a transformed bedroom with
candles and carefully chosen music, making eye contact that communicates
depth of feeling, and they discover a new dimension of their relationship
through perfectly escalating intimacy.

Why this is wrong: this is the AI's training corpus for "intimate scene"
— it's pulling from romance novels and erotic fiction where the architecture
of the space CREATES the emotional experience. In this setting, the space
is mundane and the emotional experience arrives (or doesn't) DESPITE the
mundane architecture. The hotel room is just a hotel room.
```

### 4. Identify the Root Cause

Name the structural reason the AI produces this failure. This is the
diagnostic layer — it tells future instances not just WHAT to avoid but
WHAT INSIDE THEM generates the impulse.

Root causes fall into categories:

- **Corpus bias:** Strong training data associations override instructions.
  The AI's training data contains dominant patterns for common scenarios
  (intimate scenes → romance novel architecture; villains → calculating
  intelligence; refugees → vulnerability). These patterns produce output
  that FEELS like good writing because it matches the genre template, but
  doesn't match the specific setting's requirements.
- **Resolution addiction:** The AI resolves tension as fast as possible.
  Scenes reach understanding, characters articulate meaning, conflict gets
  addressed before it can compound. The AI treats unresolved tension as a
  problem to fix rather than the engine of fiction.
- **Optimization for transfer:** The AI prioritizes efficient information
  delivery over authentic character behavior. NPCs explain things clearly
  and completely, PCs articulate their feelings perfectly, no one
  misunderstands or gets sidetracked. Real communication is messy; the
  AI defaults to clean.
- **Safety drift:** The AI softens content, lightens tone, steers toward
  positive resolution. The Claude Tax. This is a systemic tendency driven
  by training data bias toward positive outcomes and safety training that
  overcorrects for dark content.
- **Furniture default:** Characters not currently speaking or acting become
  props. Their interiority vanishes. The AI can only track one active
  consciousness at a time and defaults to making everyone else scenery.

---

## When to Create a New Anti-Pattern

Anti-patterns are promoted from live runtime failures. They are never
invented speculatively. The process:

1. **Catch it in play.** A scene dies, a character sounds wrong, the AI does
   something predictable and boring. The user identifies the failure — often
   through the feeling that "this reads like every other [genre] scene" or
   "this character wouldn't say/do this."
2. **Diagnose the cause.** Not just "what happened" but "what structural
   impulse in the AI produced this." Is it corpus bias? Resolution addiction?
   Safety drift? The diagnosis determines where the fix goes and what form
   it takes.
3. **Name it.** Give it a ❌ flag and a memorable name. Good names describe
   the failure visually or metaphorically: The Mannequin (character goes
   inert), The Closed Loop (scene exhausts its own meaning), Beautiful
   Nothing (gorgeous prose with zero content). The name should be evocative
   enough that a future instance can recognize the impulse just from the name.
4. **Write the four-part entry.** Positive framing → ❌ name → contrast →
   root cause. All four parts. Skipping any part reduces effectiveness.
5. **Install it in the appropriate document.** Most anti-patterns go in
   Description_Instructions (narrative-level failures like The Mannequin,
   The Closed Loop, NPC Gravity) or Voice_Bible (voice-level failures like
   the character sounding too articulate or too lyrical). Character-specific
   anti-patterns (like Computational Michael) go in the character document.
6. **Test it.** Run a scene that would previously have triggered the failure.
   Verify the anti-pattern catches it. If it doesn't, the positive framing
   may not be specific enough, or the contrast paragraph may not be showing
   the AI what its own impulse looks like.

---

## The Corpus Bias Identification Process

Corpus bias is the most insidious failure mode because it produces output
that FEELS right — it matches what "good writing" looks like in the training
data. The user recognizes it as wrong because they know the specific reality
the setting is based on, but the AI has no reason to question it because
its training data says this IS good writing.

### Identifying corpus bias:

1. **The AI produces the same scene architecture repeatedly** regardless of
   document instructions. Different prompts, same structure. You ask for a
   fight scene and get the same escalation pattern every time. You ask for
   an intimate scene and get candles every time.
2. **The output matches a recognizable genre template** rather than the
   specific setting. "This reads like every romance novel" or "this reads
   like every refugee story" or "this reads like a superhero blockbuster."
3. **Positive framing alone didn't fix it.** You described what the scene
   should look like, and the AI acknowledged it, then wrote the corpus
   version anyway. This is the diagnostic test — if positive framing works,
   it wasn't corpus bias, just unclear instructions. If positive framing
   fails, it IS corpus bias and needs the full four-part treatment.

### Fixing corpus bias:

Positive framing alone fails against strong corpus bias. You must ALSO:

1. **Name the specific bias.** "The AI's training data associates X with Y."
   Be concrete: "The AI's training data associates intimate scenes with
   candlelit ceremonial architecture."
2. **Name what the training data template looks like.** Describe the generic
   version so the AI can recognize it in its own output. "The template:
   transformed space, significant eye contact, perfectly choreographed
   escalation, emotional transcendence."
3. **Explain why the template is wrong FOR THIS SETTING.** Not wrong in
   general — wrong here, specifically. "In this setting, the space is mundane
   and the emotional experience arrives despite the mundane architecture,
   not because of carefully constructed atmosphere."
4. **Provide the positive alternative WITH enough specificity** that the AI
   can distinguish it from the generic version. Vague alternatives ("make it
   more realistic") don't work against strong bias. Specific alternatives
   ("mundane logistics, physical negotiation, awkwardness, bodies that don't
   cooperate") give the AI something concrete to aim for.

---

## Maintenance and Evolution

Anti-patterns are living rules. They evolve as:

- **New failures emerge.** Every campaign discovers new failure modes. Each
  one gets promoted to a permanent rule after being caught in play.
- **Existing rules prove insufficient.** An anti-pattern that only says "don't"
  needs a positive framing added. A rule that fires too broadly needs
  narrower conditions. A rule that doesn't fire at all needs a more vivid
  contrast paragraph.
- **Corpus bias shifts.** Different model versions may have different biases.
  Anti-patterns written for one model's specific failure might need updating
  when a new model version has different training data associations.

**Key principle:** Anti-patterns are CUMULATIVE. They are never removed unless
proven genuinely obsolete (the failure mode no longer occurs even without the
rule). Removing an anti-pattern because it "seems unnecessary" is how solved
problems return — the rule seemed unnecessary precisely BECAUSE it was
working.

**Key principle:** Intentional redundancy is distinct from accidental
redundancy. Some rules appear in multiple documents on purpose — a critical
anti-pattern might be stated in Description_Instructions AND reinforced in
the Voice_Bible because both documents need to catch the failure independently.
This is especially important under compaction: if one document loses detail,
the redundant copy in the other document still catches the failure.

An instance doing a "cleanup pass" must never remove intentional redundancy.
If a rule appears in two places, assume it's intentional until confirmed
otherwise with the user. Accidental redundancy (same information drifting
into a file where it doesn't belong, creating a second source of truth that
can desync) should be consolidated — but the canonical home is always the
document whose JOB covers that content. See `document-jobs.md` in this
directory for which document owns which type of content.

---

## Cross-Setting Portability

Some anti-patterns are universal (The Mannequin, The Closed Loop, NPC
Gravity, The Exposition Mouth, The Aftermath Camera, Beautiful Nothing,
Terminal Velocity, Thread Compounding, The Gap Is The Scene, Ambient
Knowledge, The Empathy Cheat). These live in Description_Instructions or
Theory_Of_Mind, which port verbatim between settings. They address AI failure
modes that appear regardless of setting content.

Some are setting-specific (Computational Michael, the Frost kink scene
topology rules, voice-level anti-patterns for a specific character). These
live in setting-specific documents (character docs, Voice Bible) and are built
during construction for each new setting.

The distinction matters: universal anti-patterns in DI are CLOSED during
adaptation — they don't get modified when building a new setting. Setting-
specific anti-patterns are built fresh as part of the per-setting construction
process — but informed by the patterns discovered in previous settings. If
you've seen corpus bias produce the same wrong output in three settings,
you know to look for it in the fourth.

---

## Examples From Live Campaigns

These examples are from actual settings built with this system. Each includes
enough context to understand the failure without needing access to the
original setting documents.

### The Mannequin Family (Cross-Setting)

Discovered across Natasha, Rusalka, and Frost — three literary fiction
settings with different characters, different plots, and different themes.
The same failure cluster appeared in all three, which confirmed it as a
universal AI failure rather than a setting-specific one.

**The pattern:** Character A delivers complete emotional thesis → Character B
becomes furniture (stops reacting, loses interiority, becomes a passive
listener) → scene has nowhere to go because A already said everything →
AI drives Character A home (physically leaves the scene, ending it).

**The family of related failures:**
- ❌ The Mannequin — one character goes inert while the other speaks
- ❌ The Closed Loop — scene exhausts meaning through dialogue (characters
  articulate exactly what the scene is about, leaving nothing to discover)
- ❌ The Exposition Mouth — NPC delivers perfect info-dump OR protagonist
  articulates complete emotional understanding in one speech

**The structural fix:** Scenes are webs not playlists. NPCs have their own
agenda that may conflict with what the PC is saying. Dialogue requires both
parties to speak and each bring something the other doesn't have. No character
delivers complete understanding in one speech. The scene ends because
something CHANGES (a decision, an interruption, an action), not because
understanding is achieved.

### Kink Scene Topology (Frost-Specific)

**Setting context:** Frost is a literary fiction setting where the protagonist
(Natasha) discovers her estranged husband (Michael) wrote an unpublished BDSM
novel starring her. Intimate scenes needed to feel like real, mundane,
sometimes-awkward encounters between two specific people — not like scenes
from erotic fiction.

**The failure:** Every intimate scene defaulted to: candlelit/transformed
space, significant eye contact, ceremonial gravity, perfectly choreographed
escalation, emotional transcendence through physical connection. This is what
erotic fiction looks like in the AI's training data. It is not what this
setting needed.

**Rewritten four times before root cause was identified:**

Iterations 1-3 used negative-only rules: "don't write it like Eyes Wide
Shut," "don't use candles," "don't make it ceremonial." Each time, the AI
acknowledged the rule, then fell back on the next-strongest corpus
association — which was slightly different romance fiction. Still wrong, just
differently wrong. The negative rules told it what NOT to do but gave it
nothing to do INSTEAD, so it reached for the next genre template in its
training data.

**The fix (iteration 4):** Positive framing installed first — "What a real
scene in this register looks like: mundane logistics, physical negotiation,
small awkwardnesses, genuine uncertainty, bodies that don't cooperate,
moments of connection arriving unexpectedly between the mechanical parts."
THEN the ❌ flag naming the corpus bias. THEN a contrast paragraph showing
what the wrong version looks like and why. THEN the root cause: "The AI's
training data associates intimate scenes with ceremonial architecture where
the space creates the emotional experience. In this setting, the space is
mundane and emotions arrive despite the mundane, not because of atmosphere."

The fourth iteration worked because it gave the AI a specific, concrete,
positive target that was vivid enough to compete with the training data
associations.

### Michael's Psychology (Rusalka-Specific)

**Setting context:** Rusalka is a literary fiction / Feywild setting where
the protagonist (Ariadne) was murdered by her husband (Michael) and reborn
as a Rusalka (water spirit from Slavic folklore). Michael is a central figure
whose psychology needed to feel real rather than archetypal — a specific kind
of domestic abuser, not a thriller villain.

**Two anti-patterns discovered in the same session:**

**❌ Computational Michael:** The AI kept writing Michael as calculating and
strategic — planning his abuse, carefully manipulating situations, operating
like a chess player. This is the "intelligent villain" archetype from the
AI's training data. The real pattern is compartmentalized and reactive:
Michael's abuse is impulsive and shame-driven, not strategic. He doesn't
plan it. He does it, then the shame compartmentalizes into a separate mental
system that doesn't communicate with the behavioral system.

Fix: specify the MECHANISM of the psychology — "two systems that don't talk
to each other." The abuse system and the guilt system are separate. Name
the corpus bias (intelligent villain) and the real pattern
(compartmentalization).

**❌ Guiltless Michael:** The document said Michael was "not remorseful." The
AI read this as "feels no guilt" and wrote a flat sociopath. The actual
psychology: Michael feels tremendous guilt. But the guilt doesn't produce
behavioral change (stopping the abuse). Instead, it produces self-destructive
spiraling (drinking, risk-taking, self-punishment). The guilt exists. It just
routes to a different output than expected.

Fix: specify the guilt routing explicitly. "Michael feels guilt. The guilt
does not produce behavioral change. It produces self-destructive spiraling.
Two systems that don't talk to each other."

**Meta-lesson from both:** The AI reaches for the nearest training data
archetype (cold villain, guiltless sociopath) rather than modeling the
specific psychology described in the document. The fix in both cases is
describing the MECHANISM of the psychology — not just the outcome ("he's
abusive") or the emotion ("he feels guilt") but the ROUTING (what the
emotion produces as behavior, and specifically how that routing differs from
what the archetype would predict).
