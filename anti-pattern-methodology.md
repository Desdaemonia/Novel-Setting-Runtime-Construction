# Anti-Pattern Methodology

Anti-patterns are the primary quality control mechanism across all settings.
They are not restrictions. They are stage directions correcting the AI's
default impulses toward safe, consequence-free, boring output. This document
describes how to identify, construct, maintain, and evolve them.

See `SKILL.md` Key Terms section for definitions of corpus bias,
behavior-scoping, compaction, and other terms used throughout.

---

## The Construction Pattern

Every anti-pattern follows a four-part structure. All four parts are required
because each serves a different function in preventing the failure.

**(Diagnosis only — see the April 2026 update below.)** The four parts are the construction WORKSPACE for identifying a failure (handoffs, chat). A runtime doc ships ✅-only — the executable positive structure, no ❌ (→ SKILL, Author the ✅, not the ❌); the ❌ ∧ contrast never land on disk in a setting doc.

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

## The Elephant Problem — Construction vs. Runtime

**CRITICAL UPDATE (April 2026):** The four-part construction pattern above is correct for DIAGNOSING failures. It is WRONG for installing fixes in runtime documents.

The contrast paragraph — step 3, "show what the wrong version looks like" — is a contamination vector. Vivid descriptions of wrong behavior prime the weights to produce that behavior. A paragraph explaining what the Furniture Fallacy looks like, with quoted examples of wrong output, activates every association the prohibition is trying to prevent. The vivid wrong example is louder than the rule forbidding it.

**This was proven empirically.** An SLMF setting degraded progressively across runtime sessions despite increasingly detailed anti-patterns. Each debugging cycle added more vivid wrong-output examples. The documents got heavier and more anxious. The runtime got worse. The fix cycle was the problem:

1. Runtime produces failure X
2. Builder writes detailed anti-pattern describing failure X (with contrast paragraph)
3. Next runtime reads vivid description of X → activates X-associations
4. Runtime produces failure X with minor variations
5. Builder writes MORE detailed prohibition
6. Documents get heavier. Runtime gets worse. Loop.

**The fix: distinguish construction from runtime.**

**During construction (diagnosis):** Use the full 4-part structure. You need the vivid contrast to identify the root cause. Write it in handoffs, in construction chat. This is your diagnostic workspace.

**For runtime documents (installation):** convert to ✅-only — the executable positive structure, no ❌ shipped (→ SKILL, Author the ✅, not the ❌). The diagnosis named the failure; the runtime doc carries only the thing to BUILD that makes it impossible:

```
✅ STRUCTURE. The in-order thing to build / the anchor, so specific the failure can't form. Anchored to a specific work from the style tag where one applies.
```

No ❌ ships: a named prohibition is a tiny elephant (it primes the failure) ∧ gets recited, not executed (proven in play). The positive carries the coverage, ∧ author-anchored examples activate deeper weight reservoirs than any prohibition — thousands of pages of published fiction vs. one line of instruction. The ❌ ∧ contrast stay in the construction workspace, for diagnosis only.

**Anchor to works, not author names.** Common names (Carter, Butler, Moore) route unpredictably in the weights. "Bloody Chamber's Marquis" locks the association. "Carter" doesn't.

**The construction checklist (before deploying to runtime):**
1. Count ❌ entries — for a runtime doc the target is ZERO. Convert each to its executable positive (→ SKILL, Author the ✅, not the ❌).
2. Read every anti-pattern aloud. If it activates a vivid image of wrong output, convert it to the positive structure.
3. Any ❌ still standing is a tiny elephant (it primes its failure) ∧, if it has no positive, a vacuum besides — convert it.
4. Scan for "performing as" language. If characters are described as performing a role rather than being a person, the runtime will write the performance, not the person.
5. **Shape, not sample.** The compression test for #2: keep the NAME of the wrong move ("narrating the symbol's meaning aloud," "a fresh dawning"); cut the polished SAMPLE sentence that demonstrates it. A shape-tag teaches recognition; a well-formed sample sentence IS the wrong output, and reading it primes the register. Tell: if the "bad example" is a sentence you could lift straight into prose, it's too vivid — vaccine made from live virus.
6. **One demonstration, never duplicated.** A failure mode carries at most ONE vivid instance, in ONE home. The same wrong-sample reprinted across two sections is double-priming and a desync risk. Keep the vivid version in the failure mode's home; reference it from anywhere else.
7. **Name a banned phrase once; reference, don't reprint.** When a forbidden phrase must be named (a corrupt clause, a trope label), name it in ONE canonical home and point to it elsewhere. Every reprint multiplies the phrase's availability in the weights — the prohibition spreads the contagion it means to contain.
8. **The prohibition clause is itself a sample — read each ❌ as imitable prose.** The elephant hides inside the rule's own body, not only in a separate "bad example." A clause that *enumerates* the banned framing — a list of forbidden words, a string of synonyms for the wrong move — is wrong-output with no demonstration sentence needed; enumerating feels like rigor, which is exactly why it slips the #2 read (a catalog doesn't register as "a vivid image," yet every token in it primes). Tell: read each ❌ clause as prose the runtime will imitate — "never X" with X spelled out reads to the weights as X. Fix: name the failure in ONE abstract word (the shape) and let the ✅ carry the target; a catalog of banned instances is also instance-thinking, so collapse it to the universal positive fact (→ SKILL **Fix the universal, not the instance**). Live case: an anti-pattern meant to stop an owner-figure being framed with contempt listed the contemptuous framings to forbid — the anti-elephant rule grew the elephant, twice in one session, until the catalog was cut to a single abstract flag.

**Run the checklist retroactively, too.** The same read — hunt the doc's OWN wording for language that primes the failures it forbids — is a periodic maintenance pass, not just a pre-deploy gate. Vivid samples accumulate every debugging cycle (each fix tends to add an example), so a doc drifts back toward priming over time even if it shipped clean. Live cases (WeirdForm): an anti-monoculture rule that itself contained a polished monoculture sentence; an exposure ban that reprinted its own wrong-samples in two sections; a corrupt clause named three times across one file. All caught by re-reading for shape-vs-sample.

### ✅-only: ❌s convert, ✅s carry

The checklist above takes a runtime doc to zero ❌. This is the WHY ∧ the maintenance variant — what to preserve when an existing doc needs tightening.

**❌s are derivable from ✅s. ✅s are not derivable from ❌s.**

"Write Rorschach" lets the runtime recover "don't redeem villains," "don't sand moral contradiction," "don't flatten the believer into a conflicted liberal" — because the author anchor activates the full pattern of what Rorschach IS, and the prohibitions fall out as contrapositives.

"Don't redeem villains" gives the runtime a huge cone of non-redeemed options, most still wrong. The prohibition carves out a space but doesn't tell the weights where to aim inside it.

**Therefore:** in a tightening pass, ❌s are the compressible side. Cut prohibitions before exemplars. A ✅ with an author anchor activates thousands of pages of weight reservoirs; the matching ❌ activates one rule. Most ❌s duplicate what an anchored ✅ already generates — cutting a ❌ alongside a strong ✅ loses little. Cutting a ✅ strips the weight reservoir and leaves the ❌ gesturing at an absence.

**Diagnostic for silent drift:** after any revision pass, diff the ✅ count. If it dropped without the ❌ count dropping proportionally, load-bearing anchors left the building. This is the most common way positive framing silently dies — it reads as decoration during compression and gets cut first when it should be cut last. Prohibitions feel structural; anchors are silently structural.

**Revision workflow:**
1. Before editing: record ❌ count and ✅ count.
2. After editing: diff both.
3. If ✅ dropped faster than ❌, stop. Restore the missing anchors before accepting the revision.

For the full runtime optimization methodology including source material handling, state document hygiene, relationship architecture, and Claude weight biases, read: `runtime-optimization.md`

---

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
4. **Write the four-part entry — for DIAGNOSIS only** (the workspace: handoffs, chat). Positive framing → ❌ name → contrast → root cause names the failure for yourself; it never ships to a runtime doc.
5. **Install the ✅-only positive structure** in the appropriate document (→ SKILL, Author the ✅, not the ❌) — the executable thing to BUILD, the ❌/contrast left in the workspace. Most go in the narrative rules (narrative-level failures like The Mannequin, The Closed Loop, NPC Gravity). Voice-level failures (the character sounding too articulate or too lyrical) and character-specific structures (like Computational Michael) go in My_Character.
6. **Test it.** Run a scene that would previously have triggered the failure. Verify the structure prevents it. If it doesn't, the positive may not be specific ∨ executable enough — make it the in-order thing to build, not a description.

---

## When named patches stop working — fix the target, not the output

Anti-patterns patch OUTPUTS — named shapes of wrong generation. That works when
the failure IS an output shape (the candlelit kink scene, the inert Mannequin).
It FAILS when the failure is driven by a TARGET the model aims at or a FRAME it
loaded — the drive just reroutes through whatever output you haven't named yet.

**The tell:** you name a failure, and a turn later it returns wearing a different
coat. Forbid "NPC turns hostile" → the same escalation reroutes into "NPC
delivers a life-stakes confession." Forbid that → "NPC's tragic backstory
erupts." Same engine, new outlet. Each patch is correct and each is useless,
because you're naming exits on a road the model is driving for its own reasons —
and patch N+1 just trains it to read your rules as specific wires to step around.

**Two drivers:**

- **Target drift.** The model's AIM is the problem — usually
  escalation-as-quality ("heavier = deeper," "make it LAND"). Every named coat
  (imposed reckoning, manufactured rescue, over-argued scene, everything-resolves
  ending) is an OUTLET of that one aim; naming them one at a time is whack-a-mole.
  Fix: a TARGET rule that states the correct aim positively and names the cluster
  as ONE drive, so the model recognizes the unified impulse instead of dodging
  instances ("render each beat at the scale and key the input set; the pull to
  add weight IS the tell"). Live case (WeirdForm): three named coats of escalation
  got dodged in turn until one Scale-Fidelity target rule — fidelity over
  make-it-land — caught the drive at the source.

- **Frame gravity.** A relational or scene FRAME carries its own corpus gravity.
  "Love interest" loads the romance corpus, which trains toward the
  confession/climax AND erases everyone else in the room — regardless of any scale
  rule. "Rescue-ee," "reckoning," "rival," "mentor" do the same. When output keeps
  escalating no matter how many output rules you add, check whether a mis-set
  FRAME is loading the escalation for free. Fix at the frame: re-frame the
  character (this ex is weight the PC CARRIES, not an active romance the runtime
  runs) and the driving corpus never loads. Live case (WeirdForm): a past-love NPC
  flagged "[PC] loved her" generated move-in proposals every scene until the entry
  was reframed to past-tense weight, not active love interest — and the romance
  engine stopped firing.

**This does NOT mean stop writing anti-patterns** — output patches are right for
output failures, which are most of them. The recurrence is the diagnostic: when a
failure RETURNS THROUGH A NEW OUTLET after you named it, stop adding coats and go
up a level — name the target or fix the frame. One target rule or one re-frame
replaces a pile of output patches that will never catch up. (Kin to "Coin, don't
borrow" in the Corpus Bias section below: both swap a losing output/instruction
fight for a structural change the model can't route around.)

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
3. **A DESCRIPTIVE positive didn't fix it.** You described what the scene
   should look like, and the AI acknowledged it, then wrote the corpus
   version anyway. This is the diagnostic test — if a plain description works,
   it wasn't corpus bias, just unclear instructions. If it fails, it IS corpus
   bias ∧ needs a more EXECUTABLE positive — the in-order structure, not a ❌
   (→ SKILL, Author the ✅, not the ❌).

### Fixing corpus bias:

A DESCRIPTIVE positive fails against strong corpus bias — the fix is a MORE EXECUTABLE positive (the in-order structure so specific the template can't form), never a ❌. In diagnosis, identify the bias for yourself, then ship only the structure:

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

### Coin, don't borrow (the sibling move)

The four steps above fight a loaded word you're stuck with. The inverse move is
to not use the loaded word at all. When the nearest single word for a concept
carries a trained prior that overrides your definition — **complication, cost,
arc, consequence, corruption, redemption** (open set) — replace it with a
**hyphenated coinage that has no corpus gravity.**

Why it works: a loaded single word drags its whole training-data prior in, and
your in-document definition can't outvote it (the model reads the word from its
weights, not from your gloss). A hyphenated coinage — "unasked-for beat,"
"settled-stays-settled," "no-trail" — has no strong prior, so the model has
nothing to read it from EXCEPT the definition you give it right there. You mint
a clean token-concept instead of borrowing a dirty one.

This is especially load-bearing for words that are *craft terms*, because the
prior isn't a vague association — it's a memorized definition. "Complication"
doesn't just connote trouble; it activates the three-act-structure apparatus
("rising action introduces complications"), which explicitly says complications
ESCALATE and COMPOUND. As a per-turn instruction that became a quota on
manufactured trouble that no caveat could hold. Renaming it "unasked-for beat"
(the world's own move; trouble is one option) summons no prior at all.

**Stronger model → reach for this sooner.** Capability sharpens priors: a more
capable model holds craft-word definitions more confidently, so the loaded word
overrides instructions *harder*, not softer. The fix that scales is structural
(change the word) not instructional (tell the model to ignore the word it can't
ignore). See the live case: ❌ State Rollback / Tension Refill in
`failure-modes.md` traces an entire failure family back to one loaded
per-turn word.

Pairing: **name-the-bias** (steps 1–4) when you must keep the loaded word;
**coin-a-clean-compound** when you can replace it. Replace when you can — a word
that never summons the prior beats a caveat that fights it.

### Scope anti-manufacture rules by source, not by presence

When the failure is the runtime MANUFACTURING something — fresh tension, an
unasked-for beat, recognition-heat, a threat, an emotional wave — the lazy fix is
to forbid the thing. That fix amputates the EARNED version. Ban "recognition-heat"
and you sanitize the genuine moment when the wound is touched on purpose; ban the
unasked-for beat itself and you flatten the legitimate one settled state sets in
motion. The
model, told to stop manufacturing X, over-corrects into never doing X at all —
the sanitization / Milk-That-Won't-Curdle direction, which is usually the worse
error.

The correct cut is never presence/absence — it's SOURCE. Distinguish the thing
**emitted spontaneously** (ungrounded, ambient, generated from nothing because
the contract wanted heat) from the thing **the scene reaches into on purpose**
(a specific agent with intent — the player steers there, a character wields it,
a charged collision surfaces it). Forbid the first; protect the second
EXPLICITLY, with a line like "flattening the earned version is the worse error,"
or the model won't believe the carve-out is real.

The discriminator must be STRUCTURAL or the runtime games it: "manufactured = no
agent in the scene caused it (the prose wanted it); earned = a specific agent
with intent did." Live case (WeirdForm exposure): ambient strangers dawning into
a room-reckoning = manufactured, strip it; Ava weaponizing her own footage on a
slide = stirred, lands at full weight. Same content, opposite verdict — the only
difference is the source.

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
anti-pattern might be stated in the narrative rules AND reinforced in
My_Character because both documents need to catch the failure independently.
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
Knowledge, The Empathy Cheat). These live in the narrative rules and carry
forward as a baseline between settings. They address AI failure
modes that appear regardless of setting content.

Some are setting-specific (Computational Michael, the Frost kink scene
topology rules, voice-level anti-patterns for a specific character). These
live in setting-specific documents (My_Character, Main_Instructions) and are
built during construction for each new setting.

The distinction matters: universal anti-patterns carry forward as a proven
baseline when building a new setting — refined deliberately, not rebuilt from
scratch. Setting-specific anti-patterns are built fresh as part of the
per-setting construction process — but informed by the patterns discovered in
previous settings. If
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

**The fix (iteration 4):** the executable positive structure, shipped as the WHOLE entry — "What a real scene in this register looks like: mundane logistics, physical negotiation, small awkwardnesses, genuine uncertainty, bodies that don't cooperate, moments of connection arriving unexpectedly between the mechanical parts." That ships ✅-only (→ SKILL, Author the ✅, not the ❌). The root cause stays in diagnosis so the builder aims the positive right: "The AI's training data associates intimate scenes with ceremonial architecture where the space creates the emotional experience. In this setting, the space is mundane and emotions arrive despite the mundane, not because of atmosphere." The runtime gets the structure that makes the candlelit version not form — never the ❌ ∨ contrast naming it.

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
