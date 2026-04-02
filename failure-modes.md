# Failure Modes — The Complete Catalog

Every rule in the setting construction system exists because a setting died
to a specific failure. This document names them, explains the root cause, and
describes the fix. These are scars, not theory.

See `SKILL.md` Key Terms section for definitions of compaction, corpus bias,
counter-torque, and other terms used throughout this document.

---

## Construction-Phase Failures

### ❌ The Monolith Failure
**What:** Creating one big document that "covers everything."
**Why it kills:** Looks complete but isn't. Every section is underdeveloped,
concerns overlap, and the AI can't tell which instruction wins when they
conflict. The document becomes a consensus document — everything at 20%
depth, nothing at 100%.
**Fix:** Multi-document architecture. Each document has one job. Build one
at a time with full attention.

### ❌ The Combination Failure
**What:** Merging documents that have different jobs. State inside instruction
docs. Instructions inside state docs. Voice rules inside world docs.
**Why it kills:** Each document tries to do two jobs; both get worse. The
instructions bloat the working content, and the working content obscures the
instructions. During compaction (when the AI's context fills and older content
gets summarized), the AI can't distinguish which parts matter for which
purpose — so everything gets flattened equally.
**Fix:** Strict job separation. Story Bible = static reference. Tracked Items
= living state. Voice Bible = how prose sounds. Description_Instructions =
how to write. One job per file.

### ❌ The Copy-Paste Failure
**What:** Taking a previous setting's document and doing find-replace.
**Why it kills:** Leaves behind artifacts from the old setting that create
confusion during play. Tracker fields for mechanics that don't exist,
character voice fragments from the source, platform-specific instructions
from the wrong platform.
**Fix:** The mapping step. Map jobs first: what carries, what transforms,
what's new, what gets cut. Build from the mapping, not from find-replace.

### ❌ The Efficiency Failure
**What:** Trying to "save time" by drafting everything at once, combining
related documents, or skipping the mapping step.
**Why it kills:** Always costs MORE time in revisions. Six documents at 20%
attention each = six documents that all need substantial rework. One document
at 100% attention = one document that's done.
**Fix:** One document at a time. Build in priority order. The "slow" approach
is consistently faster end-to-end.

### ❌ The Friendly Editor Failure
**What:** Opening Description_Instructions or Theory_Of_Mind during setting
adaptation "just to update it for the new setting."
**Why it kills:** These documents contain model countermeasures that LOOK
setting-specific but compensate for universal AI failure modes. For example,
a rule about NPC behavior might reference a specific NPC name from the
setting where it was discovered, but the underlying principle (NPCs must
have autonomous agendas) applies everywhere. Every time they get "adapted,"
load-bearing rules get removed because they look like they belong to the
old setting. The failure modes return — often not discovered until
mid-campaign when a scene dies to a problem already solved three iterations
ago.
**Fix:** Never open them during adaptation. Setting-agnostic means
setting-agnostic. If they need changes, that's a dedicated session with that
as the explicit goal, and the user makes the changes manually.

---

## Runtime / Scene-Level Failures

### ❌ The Compaction Death
**What:** Long conversations hit compaction — the automatic
summarization that occurs when the context window fills. When this happens,
detailed anti-pattern instructions, character voice nuances, and critical
rules get compressed into generic summaries that lose all specificity.
**Example:** A page of carefully crafted anti-pattern instructions for a
character named Ari — covering her specific speech patterns, her tendency
to deflect with humor, her refusal to name emotions — gets compacted into
"Ari is chaotic and genuine." Two turns later, the AI writes her as
articulate and emotionally open because the detailed rules no longer exist
in context.
**Fix:** Anti-patterns and critical rules in standalone, high-priority
documents. Voice rules in their own document at Priority 2. Document headers
and priority markers survive compaction better than mid-document detail —
the rules survive if they're in the right files at the right priority level.

### ❌ The Softening Tax
**What:** The AI pushes lighter, friendlier, removes darker themes or
language.
**Why it happens:** Training data bias toward positive resolution, safety
training overcorrection, and corpus associations between "good writing" and
"uplifting content." The AI has an inherent gravitational pull toward making
things nicer than they should be.
**Fix:** Description_Instructions contains counter-torque — explicit rules
that push against this tendency, naming what the AI will want to soften and
why softening it is wrong for this setting. Never edit DI during adaptation
(that's how the counter-torque gets lost). In setting-specific docs, name
the bias explicitly: "The AI will want to make [character] more sympathetic;
resist this because [specific reason]."

### ❌ The Mannequin
**What:** One character delivers a complete emotional thesis → the other
character becomes furniture → scene has nowhere to go → AI drives the
character home.
**Root cause:** The AI resolves dramatic tension as fast as possible. A
complete thesis IS resolution — nothing left to explore. The speaking
character has said everything the scene is "about," so the other character
has nothing to respond to except agreement or departure.
**Fix:** Structural: scenes are webs not playlists, NPCs have their own
agendas, dialogue requires both parties to speak and each bring something
the other doesn't have. No character delivers complete understanding in
one speech.

### ❌ The Closed Loop
**What:** A scene exhausts its own meaning through dialogue. Characters
articulate exactly what the scene is about, leaving nothing for action,
subtext, or the reader to discover.
**Root cause:** AI's resolution addiction. Naming the tension = resolving it.
Once a character says "the real problem is X," X is resolved as a dramatic
question — even if nothing has actually changed.
**Variants:** NPCs delivering Sherlock-level protagonist analysis ("You're
pushing people away because you're afraid of being abandoned"). Protagonist
articulating complete emotional thesis in one speech ("I realized that my
anger at him is really anger at myself"). Both cause the loop by exhausting
the scene's meaning in dialogue.
**Fix:** Dialogue implies, action reveals. No character has complete insight.
Scenes end because something CHANGES (a decision, an action, an arrival),
not because understanding is achieved.

### ❌ Beautiful Nothing
**What:** Prose that sounds gorgeous but contains no actual information. Dense
metaphor, evocative imagery, zero forward motion.
**Root cause:** The AI defaults to "literary" texture when uncertain what
happens next. It generates prose that FEELS like narrative without being
narrative — a paragraph of weather description and emotional atmosphere that
could be removed entirely without losing any story information.
**Fix:** Every paragraph must advance at least one of: plot, character
revelation, world detail, or relationship dynamic. If a paragraph could be
removed without losing anything, it should be.

### ❌ Terminal Velocity
**What:** The scene reaches its emotional peak in the first paragraph and has
nowhere to escalate.
**Root cause:** AI front-loads intensity to "hook" the reader. But fiction
requires gradation — you can't start at 10 and go to 11.
**Fix:** Scenes open at operational temperature. The emotional peak is earned,
not given. Start with the mundane logistics of the situation; let emotion
accrete through specific details and accumulating pressure.

### ❌ The Gap Is The Scene
**What:** The AI treats the ABSENCE of something (missing knowledge, unspoken
feelings, distance between characters) as the scene's engine. Scenes orbit
around what characters don't know, haven't said, or can't reach — and the
gap itself becomes the dramatic content.
**Root cause:** Related to resolution addiction but inverted. Instead of
resolving tension, the AI preserves tension by making the gap sacred —
something to be contemplated, not crossed. Characters circle the gap, the
prose describes the gap beautifully, but nothing ever COLLIDES.
**Fix:** The Collision IS the scene. Fiction lives in the moment forces
actually meet — when the thing that was unsaid gets said badly, when the
distance closes and the contact is awkward, when knowledge arrives and the
character does the wrong thing with it. The gap is the setup. The collision
is the scene. If no collision occurs, the scene is stalling.

### ❌ Thread Compounding
**What:** Every unresolved thread gets addressed simultaneously, creating a
pile-up that trivializes each one.
**Root cause:** The AI treats unresolved threads as a to-do list. When it
encounters multiple open threads, it resolves all of them in the same scene —
the character confronts their father AND makes up with their friend AND
discovers the conspiracy, all in one conversation.
**Fix:** One or two threads per scene. Other threads exist as background
pressure but don't get addressed. The tension of unresolved threads is a
FEATURE, not a bug to fix.

### ❌ NPC Gravity
**What:** NPCs orbit the protagonist like satellites. They exist only in
relation to the PC, have no lives offscreen, and never initiate without
being prompted.
**Root cause:** The AI models NPCs as response functions rather than
autonomous agents. An NPC "exists" when the PC interacts with them and
ceases to exist between interactions.
**Fix:** NPCs have agendas that exist independently of the PC. They do things
offscreen. They bring their own problems into scenes. They sometimes say
"no" or change the subject. They are not waiting in storage until the PC
needs them.

### ❌ The Exposition Mouth
**What:** An NPC delivers a perfectly structured info-dump explaining exactly
what the protagonist needs to know, in the exact order they need it, with
no personal agenda coloring the delivery.
**Root cause:** The AI optimizes for information transfer over character
authenticity. Real people don't explain things clearly and completely — they
emphasize what matters to THEM, omit what they consider obvious, get
sidetracked by their own concerns, and sometimes get things wrong.
**Fix:** NPCs filter information through their own perspective, knowledge
gaps, biases, and agendas. They omit what they don't think matters. They
emphasize what serves their interests. They get things wrong sometimes.

### ❌ The Aftermath Camera
**What:** The scene skips past the event itself and opens on the emotional
aftermath — characters processing, reflecting, discussing what just happened
rather than living through the thing that happened.
**Root cause:** The AI treats the emotional reaction as the scene's payload
and optimizes delivery by jumping straight to it. But the reaction has no
weight without the experience that generated it. The reader didn't earn the
aftermath because they weren't present for the event.
**Fix:** The camera stays in the room. The event plays out in real time with
all its mundane logistics, awkward beats, and physical reality. The aftermath
arrives AFTER the event, not instead of it.

---

## Cognition / Knowledge-Bleeding Failures

These failures relate to Theory_Of_Mind rules — preventing characters from
knowing things they shouldn't know. Like the narrative-level failures above,
these are universal across all settings because the underlying AI tendency
(leaking knowledge between characters) is universal.

### ❌ Ambient Knowledge
**What:** Characters react to information they haven't received. NPCs comment
on the PC's emotional state when they haven't observed anything that would
reveal it. Characters reference events they weren't present for. The AI's
omniscient awareness of the full story leaks into individual characters'
knowledge.
**Root cause:** The AI sees everything in the context window and doesn't
naturally distinguish between "information available to the narrator" and
"information available to this character." Without explicit rules, knowledge
flows freely between all characters because the AI models them all from
the same pool of information.
**Fix:** Theory_Of_Mind rules: a character knows ONLY what they have directly
experienced, been explicitly told, or could reasonably infer. Knowledge
transfers through dialogue, messages, and observable behavior — not through
narrative proximity or dramatic convenience.

### ❌ The Empathy Cheat
**What:** NPCs demonstrate uncanny emotional perception — correctly reading
the PC's unspoken feelings, knowing exactly what they need to hear,
understanding their trauma without being told. Characters become therapists
who happen to say the perfect thing at the perfect moment.
**Root cause:** The AI optimizes for emotional resolution and has access to
the PC's full internal monologue. When an NPC encounters a distressed PC,
the AI draws on everything it knows about the PC's psychology (from the
character documents, from the conversation history) and channels it through
the NPC's dialogue — even though the NPC couldn't possibly know any of it.
**Fix:** NPCs are bad at reading people, just like real people are. They
misread situations. They project their own concerns. They offer advice based
on their own experience, not the PC's actual problem. When an NPC does
correctly perceive something, it should be because observable evidence
supports it — not because the AI gave them a cheat sheet.

---

## Corpus Bias Failures

Corpus bias is when the AI's training data contains strong associations for
a concept that override document instructions. The output matches a genre
template from the training data rather than what the setting documents
describe. See `anti-pattern-methodology.md` (in this directory) for the full
identification and fixing process.

### ❌ Corpus Bias (General)
**What:** The AI has strong training-data associations that override document
instructions.
**Examples from live campaigns:**
- Kink scenes → operatic fantasy (candlelit rooms, ceremonial
  gravity, transcendent emotional connection) vs. the mundane reality a
  specific setting required. Discovered in Frost — a literary fiction setting
  where intimate scenes needed to feel awkward and real, not cinematic.
- Refugees → vulnerability/gatekeeper dynamics (the AI defaults to writing
  displaced characters as helpless and authority figures as either saviors
  or obstacles) vs. characters with full agency in difficult circumstances.
- "UN-style framework" → institutional process as plot leverage (the AI
  treats institutional procedure as dramatically interesting when it's
  actually just bureaucratic friction).
- Musical performance → transcendent emotional breakthrough vs. just playing
  a song that sounds good.
**Fix:** Name the bias EXPLICITLY in the document. Don't just describe the
desired behavior — name what the AI will WANT to do and explain why that's
wrong. The positive framing alone isn't enough when corpus bias is strong.

### ❌ Computational Michael
**What:** An abuser character is written as calculating and strategic when the
real pattern is compartmentalized and reactive.
**Context:** Discovered in Rusalka — a literary fiction setting where
Michael, a husband who murdered his wife, needed to feel psychologically
real rather than like a thriller villain. The AI kept writing him as a
calculating mastermind because its training data associates "abuser" with
"intelligent villain archetype."
**Root cause:** Corpus bias toward the intelligent villain archetype. Real
abusers often operate from two systems that don't talk to each other — guilt
routes to self-destruction (drinking, risk-taking), not to stopping the
behavior. The abuse and the guilt exist in separate compartments.
**Fix:** Specify the MECHANISM of the character's psychology, not just the
outcome. Name the corpus bias (intelligent villain) and the real pattern
(compartmentalization, guilt-as-self-destruction-not-behavioral-change).
**Generalizable lesson:** Whenever a character's psychology is based on a
real pattern that differs from the genre archetype, specify the mechanism
explicitly and name the archetype the AI will reach for instead.

### ❌ Guiltless Michael
**What:** An abuser character shows no guilt because the document says he's
"not remorseful," when the actual character has guilt that routes to a
different output (self-destruction rather than changing behavior).
**Context:** Same setting (Rusalka), same character. The AI read "not
remorseful" as "feels no guilt" and wrote a flat sociopath, when the
intended psychology was more complex: Michael feels tremendous guilt, but
the guilt doesn't produce the expected output (stopping the behavior). It
produces self-destructive spiraling instead. Two systems that don't talk
to each other.
**Root cause:** AI reads "not remorseful" as "feels no guilt" when the
intended meaning is "guilt doesn't produce the expected behavioral output."
**Fix:** Specify the guilt routing explicitly. "Michael feels guilt. The guilt
does not produce behavioral change. It produces self-destructive spiraling.
Two systems that don't talk to each other."
**Generalizable lesson:** When describing a character's emotional responses,
specify the ROUTING (what the emotion produces as behavior), not just the
presence or absence of the emotion.

---

## Build-Instance Failures

### ❌ The Hallucination Install
**What:** A build instance (the AI helping construct setting documents)
fabricates a detail — a childhood injury, a backstory event, a relationship
fact — and installs it as canonical across multiple document sections.
**Why it's dangerous:** Once installed in a static document, the fabrication
becomes "true" — future instances treat it as established fact and build on
it. Discovered when a build session invented a childhood injury for a
character that never happened, then referenced it in three separate document
sections as though it were source material.
**Fix:** Every detail in character/world documents must come from the user's
source material or be explicitly approved. Flag uncertain details. Review
each document against source material before approval. When in doubt, ask.

### ❌ State-as-Static Trojans
**What:** Words describing starting conditions become permanent truths the AI
refuses to contradict during play.
**Example:** "Ariadne is isolated and alone" in the Story Bible becomes an
immutable fact. Even after she builds relationships during play, the AI keeps
writing her as fundamentally alone because the static document says so. The
AI treats the Story Bible as ground truth and overrides the runtime state in
Tracked Items.
**Fix:** Audit static documents for state claims. Describe possibility space,
not current state. "Ariadne's default mode is isolation" (describes tendency
that can change) vs. "Ariadne is isolated" (describes state the AI will
maintain forever). Tracked Items is where current state lives — the Story
Bible describes only the starting position and stable tendencies.

---

## Design Principle Failures

### ❌ The Frictionless World
**What:** Narrative elements are constructed without potential for conflict,
or the AI resolves conflict before it can develop.
**Why it kills:** Narrative REQUIRES conflict — physical, emotional, social,
ideological. A setting where nothing can go wrong is a setting where nothing
happens.
**Root cause:** The AI's default is harmony. NPCs agree, systems cooperate,
relationships stabilize. Conflict feels like a problem to fix rather than
the engine of the story. This manifests during both construction (building a
world where everyone is basically cooperative) and runtime (resolving
conflicts immediately rather than letting them develop).
**Fix:** Every narrative element should carry potential for conflict without
removing user agency. NPCs have competing interests. World systems create
friction. Relationships contain unresolved tensions. The AI's resolution
addiction must be countered structurally — during construction by building
friction INTO the setting, and during runtime by the anti-patterns in
Description_Instructions.
