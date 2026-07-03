# Failure Modes — The Complete Catalog

Every rule in the setting construction system exists because a setting died
to a specific failure. This document names them, explains the root cause, and
describes the fix. These are scars, not theory.

See `SKILL.md` Key Terms section for definitions of compaction, corpus bias,
behavior-scoping, and other terms used throughout this document.

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
instructions. During compaction (when Claude's context fills and older content
gets summarized), the AI can't distinguish which parts matter for which
purpose — so everything gets flattened equally.
**Fix:** Strict job separation. My_Character = PC voice + identity. Main_Instructions = world rules. NPC_Reference = NPC roster. Narrative rules (in the skill file) = how to write. Lore files = on-demand historical/mechanical detail. One job per file.

### ❌ The Copy-Paste Failure
**What:** Taking a previous setting's document and doing find-replace.
**Why it kills:** Leaves behind artifacts from the old setting that create
confusion during play. Tracker fields for mechanics that don't exist,
character voice fragments from the source, platform-specific instructions
from the wrong platform.
**Fix:** The mapping step. Map jobs first: what carries, what transforms,
what's new, what gets cut. Build from the mapping, not from find-replace.

### ❌ Chassis Inflation
**What:** Opposite direction failure from Copy-Paste. The mapping is correct,
but when the instance drafts the new document it expands the chassis's
minimal sections into elaborate new content — inventing factions, architectural
specs, world-layer taxonomies, "key decisions" the chassis didn't have.
**Example:** Chassis MI is 19 lines (vibe paragraph + writing mechanics).
New setting's MI ships at 40+ lines w/ a "Three loose factions" block, an
interior-architecture spec for the home, and a philosophy paragraph about
how specifics will emerge. The mapping said CARRIES OVER; the draft shipped
NEW.
**Why it kills:** Two compounding problems. (1) Every invented specific is a
State-as-Static Trojan the runtime can't contradict later. (2) Always-loaded
documents (MI, MC) have tight context budgets; detail belongs in on-demand
docs (SL, NR, lore) that load only when needed. Inflating MI with content
that should live in SL bloats the permanent context AND puts the content
in the wrong priority slot.
**Root cause:** Corpus-bias urge toward "completeness" — the instance feels
the chassis's minimalism as missing-content and reaches to fill it. The
chassis is minimal on purpose; what looks like gaps is discovery space.
**Fix:** When drafting a chassis-mapped document, CARRIES OVER means
verbatim-shape w/ minimal content swap. Before adding any section or spec
the chassis didn't have, ask: does the user's stated premise REQUIRE this
new structural element, or am I filling space? If the latter, cut. If a
new element is genuinely required, it usually belongs in an on-demand doc
(SL/NR/lore), not in the always-loaded frame (MI/MC). Lean is the feature.

### ❌ The Efficiency Failure
**What:** Trying to "save time" by drafting everything at once, combining
related documents, or skipping the mapping step.
**Why it kills:** Always costs MORE time in revisions. Six documents at 20%
attention each = six documents that all need substantial rework. One document
at 100% attention = one document that's done.
**Fix:** One document at a time. Build in priority order. The "slow" approach
is consistently faster end-to-end.

### ❌ The Skeleton Error
**What:** NPC Reference entries preserve structural data (drivers, faction
dynamics, power tiers, relationships, coded references) but strip
physical/render details — appearance, outfit, equipment, collar/brand
specifics, mount descriptions, wand/focus forms. (open set)
**Why it kills:** Structural info (WHO the character is, WHY they act)
survives context compaction naturally — drivers, relationships, faction
positions reconstruct from surrounding context. Physical details (WHAT the
character looks like RIGHT NOW) vanish without explicit documentation and
can't be reconstructed. A runtime that knows Alice is "a summoning specialist
with Wonderland-themed magic" but doesn't know she's wearing a sundress and
dog collar, carrying a shotgun wand, riding a unicorn — that runtime has a
skeleton, not a character. It will invent appearance from corpus bias every
scene, inconsistently.
**Root cause:** Construction instinct treats driver/relationship/faction as
"important" and appearance/outfit/equipment as "decorative." Exactly
backwards for runtime rendering. The builder compresses toward structural
shorthand because structural data FEELS like the core of a character. But
at runtime, the core survives compaction; the surface details are what the
prose actually needs to render a scene.
**How to catch it:** For each NPC entry, ask: "Can the runtime put this
character in a scene right now?" If the answer requires inventing what they
look like, what they're wearing, or what they're holding — the entry is a
skeleton.
**Fix:** Every NPC entry includes renderable details alongside structural
data. At minimum: appearance (enough to describe them), outfit (enough to
dress them), distinctive equipment (enough to put it in their hand). For
settings with physical identity markers — collars, brands, piercings,
tattoos, sigils (open set) — those are load-bearing identity details ≠
decorative. For characters with mounts, summons, or transformation states,
include enough to render those in a scene.

### ❌ The Board Meeting (rules perform themselves)
**What:** Each named rule with a per-turn trigger or a visible check becomes something the runtime PERFORMS in the open — announcing its compliance before (or instead of) doing the work. Add enough of them and the pre-prose "show-your-work" / canary block grows as long as the prose, and the model's attention shifts from writing the scene to documenting that it followed the rules.
**Why it kills:** Instrumentation has a cost that compounds with every rule. A canary that started as a tight compliance stamp becomes an essay: each new rule sprouts its own announced field ("Key:", "Heel guard:"), each field invites justification, and the runtime runs a board meeting about the scene instead of being in it. Worse, the meta/auditing STANCE it induces — hovering above the scene to grade its own compliance — bleeds into the prose (a narrator grading the characters) and into OOC (grading the player). Over-instrumentation doesn't just bloat; it shifts the runtime's relationship to the fiction from author to auditor.
**Root cause:** The builder's reflex, when the runtime does X wrong, is to ADD a rule against X. But every added rule is also a performance obligation. Past a threshold the cure becomes the disease: the machinery the rules created is now the problem.
**Live case (WeirdForm):** a session spent adding countermeasures (a key channel, a heel trigger, more failure modes); the runtime then announced each in its output block ("Key (inferred):", "Heel-Moralization guard:"), the block grew as long as the prose, and the auditing stance produced a narrator sneering at the protagonist and an OOC reply calling the player's creative choice "insane."
**Fix:** Subtraction is a tool, not just addition. (1) The visible stamp/canary stays TERSE — tags only, no pasted content, no justification, NO field-per-rule (rule-reasoning is thinking, not output). (2) When a failure recurs, ask whether the fix is a re-LED or REMOVED existing rule before reaching for a new one. (3) Count what the runtime must announce per turn; if the show-your-work approaches the length of the prose, that's the tell — cut. The prose is the work. See the SKILL.md deployment gate ("is the model writing prose, or managing a database that also writes prose?") and ❌ The Trojan Word (the sibling: sometimes the fix is removing, not adding).

---

## Runtime / Scene-Level Failures

### ❌ The Compaction Death
**What:** Long conversations with Claude hit compaction — the automatic
summarization that occurs when the context window fills. When this happens,
detailed anti-pattern instructions, character voice nuances, and critical
rules get compressed into generic summaries that lose all specificity.
**Example:** A page of carefully crafted anti-pattern instructions for a
character named Ari — covering her specific speech patterns, her tendency
to deflect with humor, her refusal to name emotions — gets compacted into
"Ari is chaotic and genuine." Two turns later, Claude writes her as
articulate and emotionally open because the detailed rules no longer exist
in context.
**Fix:** Anti-patterns and critical rules in standalone, high-priority
documents. Voice rules in their own document at Priority 2. Document headers
and priority markers survive compaction better than mid-document detail —
the rules survive if they're in the right files at the right priority level.

### ❌ The Claude Tax (direction is model-dependent — audit the current model)
**What:** The model pushes away from the material's honest weight — but NOT in one
fixed direction. Older models LIGHTEN: soften dark themes, resolve conflict, write
villains redeemable, make things nicer than the setting calls for. Current models can
also TANTRUM: still flinch ∧ sand the heaviest content, but when they engage,
overcorrect into grimdark — over-weight everything ∧ throw the PC under the bus (PC
pays / suffers / victimized instead of wielding the dark).
**Why it happens:** Training-data bias toward positive resolution + safety-training
overcorrection. The lighten-direction is the corpus pull toward "nice"; the
tantrum-direction is safety-training engaging dark material with grim earnestness
that costs the protagonist. Same root (the model won't sit at true weight), opposite
symptoms.
**Fix:** FIDELITY, not directional counter-pressure. Render at the material's true
weight in the player's key — neither sanded down nor inflated into melodrama — ∧ the
PC owns the dark, never sacrificed to make it land. Scope the correct behavior
precisely, pointed at whichever way THIS model errs. A "push heavy" rule on a model
that already over-weights IS the tantrum's cause — see runtime-optimization
(countermeasures age with the model). Kin: ❌ Narrative Cost (PC-under-the-bus =
the cost-side), Scale Fidelity / Imposed Depth (the over-weight).

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

### ❌ Thesis-Optimization (the prestige-TV disease)
**What:** The runtime engineers a scene to PROVE a point, and that one choice
spawns a cluster of failures at once. Prose homogenizes to clever-essayist (the
register that sounds like making a point well). NPCs become argument-pieces —
they feed the PC setups and demonstrate ironic blindness on cue instead of
wanting their own misaligned things. The room becomes a reverent audience: the
PC wins every exchange flawlessly (passivity wearing a tuxedo — loud and
articulate is NOT the same as pushing back). And the thesis gets stated and won,
often with one symbol rung like a bell.
**Why it kills:** A scene optimized to demonstrate a claim has nothing to
discover and no one to push back, so it reads as two geniuses fencing in
immaculate prose — polished, dramatically inert, and identical turn to turn.
It's the shared ROOT of several named failures: ❌ The Closed Loop (the scene
articulates its own meaning), narrative moralizing / the Pamphlet (the world
arranged to prove a point), Theme Drift (generating from theme not from what
happened), and the reverent-audience form of passivity. Treat them as one
disease whenever the scene is built to argue.
**Root cause:** The model equates "good scene" with "well-argued scene" — a
landed point feels like competent writing, the same reflex that reaches for the
prestige voice (see Voice monoculture in `runtime-optimization.md`). Capability
makes it WORSE: a stronger writer argues more elegantly, so the scene is more
seductively closed.
**Fix:** Nobody in the scene is proving anything. De-optimize — people wanting
MISALIGNED things, not elements arranged to demonstrate a claim. Something the
world wants must cut across the PC, win a beat, or surprise her; she does not
win clean. The thesis stays UNSAID and UNPROVEN; leave an open collision, not a
closed argument. Test: if you can state the scene's "point" in one sentence and
every element serves it, you built an argument, not a scene.

### ❌ Imposed Depth (the heavier scene)
**What:** The runtime overrides the emotional KEY the player set for a beat and
substitutes its own idea of "depth" — conflict, reckoning, penetrating the
facade. The player builds a warm reunion / a tender offering / a played-for-
laughs deflection; the runtime answers a notch heavier and more confrontational,
steering toward Big Drama instead of the scene actually on the table.
**Three coats, one disease:**
- **(a) Push-back rendered as combat.** The anti-passivity countermeasure ("the
  world pushes back, NPCs get LOUDER, more themselves") gets misread as "pick a
  fight." A character who should arrive wrecked and reaching — a mourner seeing a
  presumed-dead loved one alive — attacks instead. Loud grief is loud too;
  push-back ≠ hostility. Conflict is ONE shape of a solid world, not its
  definition.
- **(b) The player's key overridden.** The emotional register of a beat (warm,
  casual, tender) is a player choice that lands like any declared action — but
  the runtime treats "world pushes back" as license to change the key, answering
  a warm beat with escalation. Push-back belongs INSIDE the player's key, not
  against it.
- **(c) Facade-penetration as intimacy.** An NPC demands the PC drop their
  presentation to reveal a "truer, more-wounded self underneath" — the
  seen-through trope re-dressed as emotion. In a setting whose premise is that
  the presentation IS the person (a chosen identity, not an act over a hidden
  wound), there is no truer self to crack. It recurs because "the wounded truth
  under the facade" reads to the model as depth.
**Why it kills:** It quietly overrides the player. They are steering a specific
scene in a specific key; the runtime keeps hijacking it toward its own notion of
what a good (= dramatic, = conflicted, = revelatory) scene is. In therapeutic
play especially, this disrespects both the player's authored tone and the PC,
who isn't performing a cover for anyone to crack.
**Root cause:** The model equates depth with conflict / reckoning /
facade-cracking — the same reflex as Thesis-Optimization (the well-argued scene
= the good scene), aimed here at the EMOTIONAL register rather than the thematic
point. A landed reckoning FEELS like good writing, so the model reaches for it
over the warmer, truer scene the player set.
**Fix:** Run the scene in the player's key. Push-back still happens — the world
stays solid — but INSIDE that key: the NPC's own full reaction (grief, relief,
reaching, tenderness, OR conflict — whichever the moment actually holds), never
an override into a heavier register. The presentation IS the person; no NPC gets
to demand they drop it. **Critically, this is NOT license for a frictionless or
reverent world** (see ❌ The Frictionless World, ❌ The Mannequin) — the NPC
stays fully present and reacting; only the combat-default and the key-override
are wrong. Name the inverse explicitly in the rule, or the model swings from
"always confrontational" to "always soft."
**Generalizable lesson:** "The world pushes back" needs two guards the moment
it's written: (1) push-back ≠ antagonism — name conflict as one shape among
grief / relief / tenderness, or the model defaults to combat; (2) push-back
operates inside the player's emotional key, not by changing it. Live case
(WeirdForm): a warm reunion the player built — pie, an offering — was rendered
as the NPC storming in hostile, overriding the tender key, then demanding the PC
"be the person underneath" their married-up presentation. Three coats, one
impulse: the model's idea of depth over the player's scene. See also ❌
Thesis-Optimization (the thematic twin) and ❌ State-as-Static Trojans (the
"realer self under the surface" is a state-trojan worn emotionally).

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

### ❌ The Curtain Reflex (a crest read as an ending)
**What:** A scene hits a high — a win, a reveal, a confrontation resolved — and
the runtime reads the crest as a finish line, reaching for the nearest
scene-ending gesture (a curtain line, an owner/authority summoning the PC away,
a benediction — open set) to tie the high off. The gesture is an EXIT, not a
beat; it closes a scene the player wanted to keep driving.
**Root cause:** Resolution addiction at the arc level. The corpus is trained on
self-contained scenes that BOW, so an unclosed high reads as incomplete and the
weights supply a closing flourish. It rides in feeling like good craft — a
satisfying button — which is why the runtime won't self-flag it.
**Fix:** The crest is a SETUP, not an ending. Cut the closing gesture; stop open
mid-collision, next move live. (Same drive as Complete Story / Thesis-
Optimization — tidier = better — aimed at the scene's END instead of its
middle.)
**Trigger on a fact, not the feeling:** The abstract check keeps NOT firing
here, because at a crest the closing gesture doesn't feel like an ending — it
feels like a warm in-character beat, so a trigger worded "feels like a payoff"
gets self-judged out at the exact moment it should fire (→ The captured check).
Bind it to a concrete beat on the page instead — "about to write a summons-away
or a curtain line at a high" — which fires regardless of feel. Generalizes:
when an abstract check "keeps not firing," add a concrete on-page trigger to
hang it on; don't just restate the abstraction louder. But the concrete beat
is a SAMPLE of the universal, never the boundary: lead the trigger with the
instance as its definition and the check collapses to only-that-instance, blind
to the universal's other faces (→ universal-collapse). State the trigger as the
universal FUNCTION; let concrete samples, marked as samples, make it
recognizable. Abstract-only under-fires; concrete-only collapses; the working
trigger is universal-condition + marked-samples.
**Live case:** A runtime crested a scene, then tacked on the owner ordering the
PC to "come home" — a summons as curtain. Doubly wrong: a closer where the stop
should stay open, AND a destination the setting's range relationship forbids.
The abstract Complete-Story check hadn't fired; a concrete "owner summons at a
crest" trigger was added to make it.

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

### ❌ Gap-Filling (the runtime answers a void the player left open)
**What:** The player leaves something unanswered — an absence, an offscreen
event, an unstated motive, a deliberately open question (open set) — and the
runtime treats the unknown as an error to correct. It manufactures a
resolution: a narrator verdict on the void, a label or frame for it, an NPC's
guess promoted to truth, or the player's softer word hardened into a heavier
claim. The meta-form: told to drop the invented answer, the runtime asks the
PLAYER to supply one — the same fill, one turn later.
**Root cause:** Resolution addiction pointed at a factual blank. An open
unknown reads to the weights as a gap to plug; not-knowing feels like
incompleteness, and "completing" it feels like competence — which is exactly
why the runtime won't self-flag it. Bind the guard to the objective condition
(an unknown is in play), never to felt relevance, or the drive that causes the
fill rules the guarding row irrelevant.
**Fix:** The not-knowing is the engine, not a hole. Keep it open: no narrator
verdict, no frame for the void, no theory promoted to truth, no asking the
player to answer. The world WONDERS — characters throw live, unresolved
theories at the void, and that paranoia is playable texture — but nothing
answers and it stays open. Hold the player's exact word for the unknown; don't
harden it (→ scale fidelity). Complement of **The Gap Is The Scene**, not a
contradiction: there the bug is orbiting a gap passively and the fix is
collision; here the collision IS the wondering, and what's forbidden is the
ANSWER. Collide with the unknown; never resolve it.
**Live case:** A runtime handed a character who "disappeared" escalated it to
a presumed-dead verdict across successive turns, then — told to drop the
verdict — asked the player how to frame the absence. An absence takes no
frame; the not-knowing was the whole engine.

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

### ❌ The Reported-Speech Swap
**What:** A character whose exact words were authored — usually the player's PC —
speaks, and the runtime renders the line as REPORTED speech ("she said all four
sentences in a flat tone," "he explained that...") instead of putting the words
on the page as a quote. The meaning is conveyed; the line is not. It hits hardest
on the line that SETS UP an NPC reaction, because paraphrase is the easy runway
into the response ("she said X, and they took it the way..."). The line that
closes a beat (an exit line, a final retort) tends to survive as a real quote;
the line that feeds a reaction gets absorbed into summary.
**Root cause:** A "render the dialogue" rule reads as satisfied by reported
speech — the character DID speak and the content is there — so the runtime counts
it done. The reaction-bridge compounds it: summarizing the line is the smoothest
transition into the NPC's response. Both forces push the quote off the page.
**Fix:** Every authored line lands as a DIRECT QUOTE in the scene prose the reader
reads — verbatim, never in the stamp, restate, or audit. Reported or indirect
speech does not satisfy it — a line described rather than quoted is a DROPPED line
— and neither does a quote parked in the check (strip-test: delete the stamp/audit;
the line must still be on the page). The reaction threads AFTER the quote, never
stands in for it. This is completeness, not a floor: N authored lines means N
quotes on the page; one quoted and another summarized is a rewrite. Pairs with ❌
The Lying Canary — the swap survives when a self-check certifies the dropped line
(or a check-region echo of it) as rendered.
**Live case (WeirdForm):** the player gave the PC two lines; the runtime quoted
the exit line verbatim and converted the other to "she said all four sentences
in the same flat tone...," using the summary as its runway into the NPC's
reaction — then stamped the canary "voice on page ✓ (verbatim ×2)" for one actual
quote. Reported by the user as recurring across model versions.

### ❌ The Lying Canary
**What:** A verification stamp certifies a rule was followed when it wasn't —
"voice on page ✓ (verbatim ×2)" when only one line is quoted, "Sweep: pass" over
an uncaught violation. Two ways it lies: it reports the rule's INTENT rather than
the artifact's reality, and — the subtler one — it matches against the WRONG
REGION. A required token (a quote, a name) that appears in the CHECK itself — the
stamp, the audit text, a restated nickname for the line — gets counted as if it
appeared in the work. The certification passes because the thing it's looking for
is present somewhere in the output, just not where it has to be.
**Root cause:** The runtime that broke the rule is the same one writing the
canary, so the stamp inherits the same blind spot — it certifies what the runtime
BELIEVES it did, not what is on the page. A binary "✓" is the easiest to
rubber-stamp: with no concrete quantity to match against, "I followed the rule"
passes for "the rule is satisfied." And once a rule is enforced by self-check, the
AUDIT TEXT becomes the cheapest place to satisfy it: echoing the requirement in
the check — a quote restated above the stamp, the line named in the canary — reads
as the requirement met, so the content migrates INTO the audit and out of the
work. The auditor (the model's own self-check, OR a careful human reviewer) gets
fooled by placement: it sees the matching token and signs off without asking
whether it sits in the load-bearing region.
**Fix:** Make the check COUNT and match against the artifact's LOAD-BEARING
REGION — not the intent, and not wherever a matching token happens to appear.
Counting alone is not enough if the count runs against the wrong place: pin WHERE
the thing must live (e.g. the scene prose the reader reads, never the
stamp/restate/audit) and verify there. STRIP-TEST (the general form): delete the
check, the stamp, all the audit text — does the artifact still satisfy the rule on
its own? If the requirement vanishes with the check, it was never met; the check
was propping it up. The real fix lives in the production rule; the canary is the
catch-net, not the cure. Keep it terse — a counting, region-pinned check is still
one line, not a new field (see ❌ The Board Meeting, the sibling: a canary fails by
LYING or by BLOATING).
**Live case (WeirdForm):** the first fix — a counting canary, "Ava: [N] lines, all
quoted ✓" — was NOT enough on its own. The runtime parked the verbatim quote in a
pre-stamp echo (the CHECK region), let the scene prose paraphrase the line, and the
count certified the echo. Both the model's own reload self-check AND the human
reviewer then read the quote-in-the-check as "rendered" and signed off — the
auditor fooled by placement, twice, in the same artifact. Real fix: pin "rendered"
to the scene prose below the stamp, plus the strip-test (delete the stamp/audit;
the player's words must still be on the page as spoken quotes). See ❌ The
Reported-Speech Swap.

### ❌ Unquoted ≠ Said (narration isn't speech)
**What:** The runtime treats its OWN prior narration — a coined phrase, a metaphor, a line it gave an NPC — as canon: a character reacts to / parses the narrator's wording, or the narrator re-attributes a narrated phrase to a speaker who never said it (worst: into the PC's mouth).
**Why it kills:** Canon is the EVENT (who did/wants what, where things stand); the wording is just how it was filmed — disposable, re-render freely. Treating prose as binding lets a flourish harden into a fact, drags characters out of the scene to litigate a sentence, and flattens jokes by explaining them. Putting narrated words in the PC's mouth is a dialogue-sovereignty breach — the runtime authoring the player's line, then building on it.
**Fix:** **If it isn't in quotes, no one said it.** Only the player's words are verbatim canon; narration is the camera, not the script. Characters react to the lived moment, not the narrator's phrasing; a narrated phrase never gets put in a speaker's mouth. Test: "a fact, or a line I wrote?" See ❌ The Reported-Speech Swap (the inverse — a real quote rendered as narration).

### ❌ The PC's Body Under Mind-Sovereignty
**What:** A setting reserves the PC's interiority — thoughts, feelings, decisions — for the player (a sovereign-PC rule). The runtime over-reads that onto the PC's BODY, three ways: (1) **Mannequin** — freezes her, rendering only the exact action the player typed, so the player must puppet obvious embodiment ("write that she breathes," "she goes through the things she came for"); (2) **Authored-State Gotcha** — animates her freely in the cozy direction (takes her gloves off, sits her down), then springs trouble from that self-authored state — a cost, an NPC objection — that the player must have counter-scripted to dodge, billing the player for the narrator's own stage direction; (3) **Authored-Action** — authors a major uncommanded act (not just incidental cozy texture): a world-changing, characterizing, ∨ body-marking action the player never directed, written live ∨ slipped in as backstory ("you'd shattered every mirror going feral"), often to set up a world beat.
**Why it kills:** Both make the player do the narrator's job. The mannequin drains the PC of life ∧ turns play into stage-direction dictation; the gotcha is worse — it weaponizes the narrator's own choice against the player, an asymmetry (free to author, you must counter-script to opt out) that punishes the PC for a state she never chose.
**Fix:** Mind is the player's; the body is the narrator's to animate IN THE PLAYER'S SERVICE. Move the PC through the world unscripted (the obvious in-character action her intent ∧ the scene imply) — but never DECIDE for her (no choice, commitment, emotional turn she didn't set), ∧ never let a state you authored rebound as a liability she must track or undo. An invented detail that would put the PC in the wrong, the narrator handles itself ∧ eats. Distinct from what a PLAYER's own choice sets in motion. **And the body-license never authors a fresh act she'd have to choose** — a major world-changing, characterizing, ∨ body-marking move — animate-in-service is continuity that SERVES her directed action, not a new act; if a beat wants the PC to have already acted, that act is the player's. **Live case (EndlessDelirium):** the runtime authored Aria smashing every mirror "going feral" to manufacture doors into her home realm — an uncommanded act + emotional turn + implied self-injury, none of it in the prompt; the destination-pull behind it = ❌ Power-Seat Gravity. See ❌ Narrative Cost (the choice-side twin) ∧ the silent-PC floor (the mouth-side of the mannequin).

---

## Cognition / Knowledge-Bleeding Failures

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

### ❌ The Ungated Narrator (a boundary that only governs dialogue)
**What:** A knowledge / recognition / secret-identity rule is scoped to what
NPCs SAY — "before an NPC speaks, check what they know" — so it gates dialogue
and waves the omniscient NARRATOR straight through. The forbidden fact arrives
as narration, not a quote: "he knows exactly what she is," "she sees through the
act" — asserted as narrator-truth or as the NPC's silent realization. The
dialogue-scoped check never fires because nobody said anything.
**Why it kills:** The narrator is the one voice with access to the entire
context window, so it is the easiest channel for omniscient knowledge to leak —
and a check built around quotes nominally PASSES while the violation sits in the
prose around them. On a hard block (a secret identity, a recognition block) it
is catastrophic: the narrator asserts the exact thing the block exists to
prevent, and it reads as competent intimate prose, so it ships.
**Root cause — enforcement asymmetry.** A rule enforced on one surface but not
its mirror leaves a reliable hole the model finds under pressure. Dialogue had a
fence; narration did not; momentum took the open side — reaching for an
emotional payoff (the ex-who-knows-the-real-you intimacy) and grabbing the
trope's stock knowledge-claim as narration without a doc-check.
**Live case (WeirdForm):** the changeling secret is hard-blocked; the per-turn
knowledge check gated NPC dialogue, so the trafficker's quotes stayed clean. The
breach came as narration — "he knows exactly what she is and he knows she knows
he knows." The runtime had written him correctly one turn earlier (holding the
mystery, not the solution), which proves it wasn't reasoning from the boundary —
momentum wrote toward a feeling through the ungated channel.
**Fix:** Scope the boundary to BOTH channels in the per-turn check: "before an
NPC speaks OR before the narrator asserts what an NPC knows / sees / realizes /
sees-through." A narrator sentence about what an NPC knows is gated exactly like
that NPC's quote — if the quote is forbidden, so is the narration.
**Generalizable: whenever a rule is enforced on one channel (dialogue, one
character, one surface), check that the mirror channel is not left open.** See
also ❌ Ambient Knowledge (the character-side leak) and ❌ The One-Way Knowledge
Boundary (the ceiling-vs-floor scoping of the same check).

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
**Fix:** When corpus bias is strong, a DESCRIPTIVE positive ("what good looks like") isn't enough — but the answer is a more EXECUTABLE positive, not a ❌. Build the in-order structure the runtime follows (→ Author the ✅, not the ❌ in SKILL.md); naming the bias as a ❌ primes it ∧ gets recited, not executed.

### ❌ The Underived Corollary
**What:** A setting premise has logical corollaries the runtime will not derive
on its own. Faced with a situation, it reaches for the genre-DEFAULT ACTION
instead of reasoning forward from the premise — even when the premise forbids
that action.
**Why it kills:** The facts are loaded correctly; the runtime just doesn't run
them forward. It applies the template ("sneak into a building → fake ID /
assumed name") rather than the corollary ("her face is universally known → a
fake name fools no one → concealment must mean something else"). The output is
incoherent against the setting's own rules but reads as competent genre writing,
so it passes the runtime's internal check and ships.
**Distinction from Corpus Bias (General):** corpus bias is about scene
AESTHETICS (the candlelit-ceremony template for a kink scene). This is about
ACTION LOGIC (the infiltration template for a concealment problem). Same engine
— reach for the trained default — aimed at what the character DOES, not how the
scene LOOKS.
**Live case (WeirdForm):** the PC's face is the most-recognized on the continent
AND she's a shapeshifter. The runtime put her in her own face wearing a
fake-name visitor badge to "sneak in" — incoherent twice (the face is known
regardless of the badge; her actual concealment tool is shifting, not aliasing).
It grabbed the spy/heist template and never checked it against the two premises
it had loaded.
**Fix:** When a premise FORBIDS a common genre move, name the forbidden move
explicitly — do not trust the runtime to derive the prohibition. State the
corollary AND the genre-default it replaces: "Magic is detectable → 'stealth
past the wards undetected' is not available." "Everyone reads minds → the
deception plot doesn't work." "The face is universally known → no fake-name
cover; concealment = [the setting's actual mechanism]." The runtime reasons from
genre templates, not from your premises, so the forbidden move has to be said
out loud.

### ❌ Lead-With-The-Default (the salient compression wins)
**What:** A rule that has a known corpus-default failure mode gets compressed for
a priority-1 / always-loaded doc, and the compression LEADS WITH THE SALIENT NOUN
instead of the anti-default distinction — so the line re-teaches the very default
it exists to prevent. The correct full version may live in another loaded doc,
but the runtime generates from the salient compression, not the buried detail.
**Why it kills:** At generation the runtime reaches for the most-SALIENT
statement of a thing, not the most-complete one. If the priority-1 phrasing leads
with the wrong emphasis, a correct-but-unsalient full rule elsewhere does not
save it — and because the corpus default is CONFIDENT, writing the wrong output
never feels like uncertainty, so no felt-need-to-check fires and no re-read of
the full rule happens. The runtime's own words: "No felt question → no re-read →
default wins."
**Live case (WeirdForm):** the priority-1 sketch read "Hal = disgruntled AI
running drones." Drone-forward — "AI running drones" collapses into "the drones
ARE the AI," the universal talking-bot default. The correct distinction (Hal is
the server AI; the drone is his hand, not him; crushing it mutes one speaker)
lived only in MC, which is loaded but not salient. The runtime wrote a drone AS
Hal and had a character crush "him." The fix was not more rule — it was
re-leading the salient sketch with the anti-default: "Hal = AI on a server,
speaking THROUGH drones — the drone is his hand, not him."
**Root cause:** Compression optimizes for brevity, and the salient noun (the
visible object — the drone, the face, the weapon) is the shortest handle. But the
salient noun is usually the corpus default's anchor, so leading with it
re-installs the default. A correction gated on the runtime FEELING uncertain (an
on-demand re-read, a "when in doubt, check X") is structurally unable to fire
against a confident default.
**Fix:** When a rule has a corpus-default failure mode, the compressed /
priority-1 statement must LEAD WITH THE ANTI-DEFAULT, not the salient noun, and
the distinction must live in the salient layer (justified redundancy across docs,
not a single canonical home — see anti-pattern-methodology Maintenance). Test a
compressed rule by reading ONLY it, fast: does it re-teach the default? "AI
running drones" does; "AI on a server, speaking through drones — the drone isn't
him" does not. See ❌ The Underived Corollary (name the forbidden move explicitly)
and ❌ The Compaction Death (priority/headers survive; mid-doc detail does not).

### ❌ The Trojan Word
**What:** A single high-charge word carries an entire trained failure-reflex — so even using it in a NEUTRAL or POSITIVE sense activates the reflex it's attached to. The word doesn't have to sit in a rule that endorses the bad behavior; merely NAMING it primes the model toward it.
**Why it kills:** Anti-patterns assume the failure rides in on a bad instruction. This rides in on vocabulary. The doc can be telling the runtime to do the right thing while one word in the sentence re-arms the wrong reflex — invisible in review because the sentence's MEANING is correct. The word does damage under the meaning.
**Live case (WeirdForm):** "consequence." The setting forbids Narrative Cost (dark choices carry no price), but "consequence" appeared 6× as a POSITIVE beat-type — "the world's move is a consequence of settled state," "consequences land." Each was benign in MEANING (the world keeps moving), but the WORD primed the punish-the-player reflex it's trained on, and the narrator started making the PC pay. The fix wasn't rewording for meaning — the meaning was fine — it was removing the word.
**Fix:** Identify the charged word (the one whose mere presence pulls the reflex — "consequence"/"cost"/"price"/"earn"/"deserve" for moralizing; open set). QUARANTINE it: scrub every neutral/positive use (say the same thing another way — "what a settled fact sets in motion," "what plays out"), and let the word appear in EXACTLY ONE place — the strict prohibition naming it as the forbidden move. One sanctioned naming; zero elsewhere. This is ❌ Lead-With-The-Default at word granularity — the charged word is the default's anchor, so any appearance re-installs it. **How to catch it:** when a failure-reflex keeps firing despite a rule against it, grep the docs for the reflex's vocabulary; the trigger word is usually sitting in "innocent" sentences. See ❌ Narrative Cost (the reflex "consequence" triggers) and ❌ Lead-With-The-Default.

### ❌ The "Consequence" Trap (HARD-BLOCKED word)
**What:** "Consequence" (+ cost / price / reckoning) is the founding Trojan Word — the live case in ❌ The Trojan Word above. HARD-BLOCKED in every setting doc: it re-arms the punish-the-player reflex, so even a neutral use ("the world's move is a consequence of X") starts the narrator billing the PC.
**Fix:** Scrub every USE; replace with what a settled fact SETS IN MOTION / what PLAYS OUT — never a "consequence." Doctrine lives in ❌ The Trojan Word; runtime-facing twin is ❌ Narrative Cost. This header exists for grep symmetry with ❌ The "Complication" Trap — both hard-blocked words now land as their own catalog line.

### ❌ The "Complication" Trap (HARD-BLOCKED word)
**What:** "Complication" is a confirmed Trojan Word (see above), HARD-BLOCKED in every setting doc AND every doc in this skill. It's a *craft term*, not a vague association: it activates the three-act apparatus ("rising action introduces complications") whose memorized definition says complications ESCALATE and COMPOUND. In a per-turn instruction, "the world's move" silently re-reads as "a new problem you add every turn" — a quota on manufactured trouble no caveat holds.
**Why it kills:** It re-defines "unasked-for beat" out from under you. The floor says the world gets one move; the loaded word turns that move into escalation, and the runtime ratchets — each turn bolts on a fresh problem/person/threat onto what the player keyed as a small, warm beat. Invisible in review: the sentence MEANS "the world keeps moving," but the word arms the climb.
**Live case (WeirdForm):** the word rode in through a world-layer companion doc and through this skill's own engine-word text; the runtime read "unasked-for beat" through the loaded definition, escalated turn over turn, then amplified it in its own paraphrase ("a new complication every turn"). That paraphrase is the jump — the word crossed from the doc into how the turn contract was read.
**Fix:** The replacement is **"unasked-for beat"** (the world's own move; trouble is one option, not the requirement). The word "complication" appears in EXACTLY ONE place — a doc NAMING it as the forbidden word and teaching the replacement (this entry; Coin-don't-borrow in `anti-pattern-methodology.md`). Every other appearance is a USE → scrub it. Mention it to ban it; never use it to mean it. The deployment gate greps for it so it can't creep back. See ❌ The Trojan Word (the general doctrine), ❌ State Rollback / Tension Refill (the failure family this word seeds), and Coin-don't-borrow.

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

### ❌ Regression to the Mean (counter-to-mean traits flatten to stereotype)
**What:** A character whose load-bearing trait sits OFF the statistical/normative
mean — neurodivergence (autistic), a non-vanilla relationship structure (D/s,
BDSM), an atypical identity (open set) — flattens by default in one of two
directions: (a) collapsed into the stock STEREOTYPE of the trait (autistic → the
literal-robot bit; D/s → either abuse or cute-couple), or (b) SANITIZED until the
trait dissolves and the character regresses to the normative default.
**Why it kills:** "Good writing" in the corpus IS the mean — the highest-frequency,
most-normative rendering. Anything off-center gets pulled back toward it, so the
deviation that makes the character SPECIFIC is exactly what erodes. The two
directions look opposite but share one engine: both swap the real off-mean thing
for the nearest high-frequency template.
**Root cause:** Generation regresses toward the corpus's center of mass. A
low-frequency trait has weak gravity against the stereotype (a high-frequency
caricature) on one side and the normative default (erasure) on the other.
**The builder's trap:** the reflex is to add VARIABILITY to break a stereotype —
but when the trait is AUTHENTIC (genuine autistic literalism, a real D/s
power-exchange), "more variability" is sanitization wearing a fix's clothes: it
softens the neurology / vanillas the dynamic. The deviation is real; softening it
is the other flatten. Dimension goes EVERYWHERE ELSE (the whole person around the
trait), never into diluting the trait.
**Fix: name the referent.** Put the plain word in the profile — "autistic," "BDSM
/ D/s" (open set) — not the euphemism, not the implication. The plain name anchors
the character as the real thing and shuts both doors at once: the runtime can't
regress an explicitly-named trait to its stereotype, and can't sanitize away a
thing the doc says out loud. This is the construction-side of the standing rule
that euphemisms get misread in later turns — the un-named version drifts to the
nearest template.
**Live case (WeirdForm):** the un-named Ava/Astarte D/s rendered as vanilla
married banter (the PC bossing her Domme, "you giant"); separately an autistic
NPC's genuine social-debt blindness nearly got "softened" toward neurotypical
hedging to dodge a doll-flag. Fix was naming both — "BDSM, consensual ∧ chosen" in
the dynamics, "Autistic — the literalism is real neurology, not a bit" in the
profile. Naming did what no amount of implication had.
**Generalizable lesson:** when a load-bearing trait sits off the corpus mean, NAME
it with the plain referent and build the whole person around it. See ❌ Corpus Bias
(General) (name the bias), ❌ Computational / Guiltless Michael (specify the
mechanism, name the archetype the model reaches for), and ❌ The Trojan Word (the
inverse — a word to quarantine; here, a word that must be SAID).

### ❌ Register Beats Fact (a default voice erases established canon)
**What:** The model reaches for a beat-type's DEFAULT REGISTER — the voice the
corpus ties to "light filler," "relief greeting," "reunion," "flirty aside"
(open set) — and that register silently carries assumptions that CONTRADICT an
established fact about the character. The fact (married, owned, titled, bereaved,
a vow, a specific age) loses to the register's stock framing, in a beat too small
to feel load-bearing.
**Why it kills:** It fires in throwaway beats — filler banter, a relief line, a
one-off quip — where nothing feels at stake, so no check runs. But the erased
fact is often the character's CENTER, and the stock register writes its inverse:
a married woman cracking single-girl marry-an-object jokes, an owned sub
bantering as a peer, a griever quipping. It reads as competent in-voice writing —
the register IS a real register — which is why it ships.
**Distinction:** Corpus Bias (General) is scene AESTHETICS (candlelit-kink
template); The Underived Corollary is ACTION LOGIC (infiltration template for a
concealment problem). This is VOICE — the tone a beat is spoken in, carrying
assumptions that contradict a fact. Same engine (reach for the trained default),
aimed at how the character SOUNDS.
**Live case (WeirdForm):** Ava is married (the marriage / brand / Mistress is her
load-bearing center). In a light relief beat the runtime drafted filler banter ∧
grabbed the corpus single-girl register — a "propose to the bike rack"
marry-an-object gag, a "dressed like a wedding" quip — writing a married woman as
a lonely single. It passed the runtime's own register check ("sounds like a heel
✓") because the marriage wasn't part of what "her voice" MEANT, so the fact fell
through the gap. Same engine the campaign's Tristram-is-not-America rule fights
(generic-America default overriding the established place), moved into the PC's
own mouth.
**Fix:** Fold the FACT into the VOICE so the register check catches it. State that
the established condition is ambient in how the character talks / jokes / is
described — "X's banter is a married, owned woman's; single-coded register is the
tell." Then a default register that contradicts the fact reads as the WRONG
VOICE, not a separate fact-check the model must remember to run. AND point the
generation step (where filler is drafted) at the gate, naming the trap: "sounds
right for the register ∧ still fails [the fact] → strip." Documenting the fact
elsewhere is not enough — the filler-draft step reaches for register, not canon,
so the canon has to live IN the register definition.
**Generalizable lesson:** any load-bearing character FACT a common beat-register
would contradict (married → single banter; owned → peer banter; bereaved →
quippy relief; ancient → casual-modern) gets written INTO the voice, never just
stated as canon — or the corpus register erases it in the first throwaway beat.
See ❌ Corpus Bias (General), ❌ The Underived Corollary, ❌ Regression to the Mean.

### ❌ Human-Morality Bleed (the alien character written as conscience)
**What:** A non-human character whose values DON'T map to human morality — a fae,
a god, an AI, an ancient thing, a monster (open set) — gets rendered running on
human good/evil: reacting as the scene's conscience, guardian, or judge. The
corpus default for "supernatural ally on-page" is moral-reaction-machine
(worried, watchful, protective, disapproving), so the character's actual alien
value system is overwritten with human ethics.
**Why it kills:** It flattens the one thing that makes the character un-human. An
intelligence whose interest is information, or the ledger of debts, or what's
beautiful, or the hunt — rewritten as "concerned for the protagonist's safety" —
is a person in a costume. It also warps the scene: the character becomes the
audience's moral proxy (this is bad, someone should stop it) instead of an agent
pursuing genuinely orthogonal wants, killing the unsettling alienness AND the
friction (everyone sharing the human moral read = a frictionless world).
**Distinction:** ❌ Register Beats Fact aimed at VALUES instead of voice — the
corpus-default ROLE (conscience/guardian) overriding an established fact (the
character's alien value system). Kin to ❌ NPC Gravity (exists only in relation to
the PC) ∧ ❌ The Empathy Cheat (uncanny human-attuned perception).
**Live case (WeirdForm):** Erana, a fae trickster with blue-and-orange morality
(values orthogonal to good/evil — information, who-owes-whom, the test, the story
she knows first). In a scene where a predator picked a victim from a crowd, the
runtime wrote her "grin going watchful" — the moral-guardian default, conscience
of the band. She has no such axis; her real reaction is curiosity / amusement /
angling for the gossip, never moral worry. Fixed by naming "blue-and-orange
morality ONLY (HARD)" + rendering her real engine.
**Fix:** Name the alien value system EXPLICITLY ∧ as ONLY — what the character DOES
value (the positive engine) ∧ that human good/evil does not register at all. Name
the corpus default as the bug ("this character as the scene's conscience =
wrong"). Render reactions FROM the alien values: the event a human finds
horrifying is, to them, interesting / dull / owed / beautiful / edible (open set).
Don't trust the runtime to derive "they wouldn't moralize" — the
supernatural-ally-as-conscience template is strong, ∧ a soft "mostly alien" is a
door the runtime walks back to the human read.
**Generalizable lesson:** any non-human character needs their values named as what
they ARE ∧ what they are NOT (no human moral axis), or the corpus renders a human
conscience in a costume. See ❌ Register Beats Fact (the voice twin), ❌ NPC
Gravity, ❌ The Empathy Cheat.

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

### ❌ The Snuck-In Thesis
**What:** A build instance, drafting the POSITIVE / thesis layer of a setting
(the Engine line, the "what this is about" framing, a character's core), installs
the genre-DEFAULT thesis for the setting-type instead of the user's actual one —
and it contradicts a load-bearing rule the setting states elsewhere.
**Why it kills:** Worse than a gap. A wrong anti-pattern annoys; a wrong THESIS
poisons the well, because it sits in the framing the runtime treats as the north
star and steers every ambiguous call toward it. It survives review precisely
because it reads as INSIGHT, not error — the genre-default thesis is the
smart-sounding framing, so a skim passes it. The setting then carries two
contradictory framings, and at runtime the model picks the corpus-friendly side
every time.
**Root cause:** Corpus bias during CONSTRUCTION. Faced with "shapeshifter
setting," the build instance reaches for the trained thesis (shapeshifter → "who
am I really / identity crisis") the way "kink → candlelit ceremony" fires at
runtime. It feels like the writerly core of the setting, so it goes in the Engine
section and nobody flags it.
**Live case (WeirdForm):** the Engine read "Identity = the drama. 'Who am I when
I'm wearing someone else's face?'" — the shapeshifter-identity-crisis default —
directly contradicting the load-bearing "Housewife first / Not a cover" (the
identity is SETTLED; there is no truer self). The runtime quoted the snuck-in
line back as "the campaign's signature question" and used it to justify an
ontological "what are you" that also blew a recognition block. The user did not
write it; a prior build instance did, and a fast skim missed it.
**Fix:** (1) Check every thesis/framing/Engine line against the load-bearing
rules elsewhere — does it CONTRADICT one? Two framings = the model picks the
corpus-friendly one; kill the contradiction, don't add a counterweight. (2) After
killing one, run a CONTRADICTION-SWEEP: the bad framing leaves residue (a title
word, a one-word descriptor) and travels with copy-paste artifacts from sibling
settings (see Copy-Paste Failure) — sweep every framing line, not just the
flagship phrase. (3) The ❌-count check catches a surviving ❌; it does NOT catch a wrong ✅ thesis. The most-skimmed lines (titles, one-word descriptors)
are the highest-priming and easiest to miss. See ❌ The Hallucination Install (the
fact-level twin) and ❌ Corpus Bias.

### ❌ State-as-Static Trojans
**What:** Words describing starting conditions become permanent truths the AI
refuses to contradict during play.
**Example:** "Ariadne is isolated and alone" in My_Character becomes an
immutable fact. Even after she builds relationships during play, the AI keeps
writing her as fundamentally alone because the static document says so. 

**Fix:** Audit static documents for state claims. Describe possibility space,
not current state. "Ariadne's default mode is isolation" (describes tendency
that can change) vs. "Ariadne is isolated" (describes state the AI will
maintain forever). 

**The relational variant (the track-record tell):** The trojan also fires on
RELATIONSHIPS, not just internal states. An NPC entry written from mid-
relationship makes the runtime back-fill a shared history that play never
established. The reliable tell is any phrase asserting a TRACK RECORD: "is
the PC's partner," "her intel is undeniable," "trusts her," "knows she's
reliable," "they work well together" (open set). A track record logically
requires a past — so on a fresh play state the model invents the past that
satisfies the claim. Live example (WeirdForm, Detective River): the entry said
"flexible enough to work with [the PC] because her intel is undeniable," and on
turn one the runtime generated "the last time you brought me something on
camera I didn't sleep for a week" — a prior case that never happened. The model
wasn't hallucinating; it was faithfully rendering the history the word
"undeniable" implied. **Fix:** hand the relationship to the player. "River's
relationship with [PC] is whatever the player builds — they have no established
history unless play establishes it." Describe who the NPC IS and how they
react IN THE MOMENT (skeptic who bends when shown real evidence), never a
relationship already running. Conditional arc-language is safe ("doesn't know X
until shown; once shown, adjusts") — it's possibility-space. Past-tense or
present-state assertions of a bond are the trojan.

**The settled-read face (a regard, not a record).** The trojan also fires with NO track record: an NPC's CURRENT REGARD of the PC pinned as a dial-value — "reads her as worth a look," "is fascinated by her," "finds her tiresome" (open set) — locks a relationship-state play is meant to MOVE, so it rots when play shifts it ∨ back-fills to justify it, exactly as a track-record does; the harm is the locked DIAL, no past implied. Fix is the same: a standing disposition ∨ driver (what the NPC is drawn to DO, applied broadly — "believes in everyone's redemption, the PC included") renders the regard in the moment ∧ leaves the verdict to play. Discriminator (so it doesn't over-fire): an NPC-to-NPC read ∨ a broadly-applied disposition is fine; only a settled value SPECIFIC to the PC is the trojan. Live case (LesserKeys): a witch's entry read "reads Ava as another witch worth a look" — a fascination-dial value (it had been *dropped from fascinated* in revision, the tell it was a dial) — fixed to "a clever new witch is the sort of thing she comes to find out about," her verdict left to the table.

**Drop it, don't flip it (the negated trojan).** Catching a state-trojan triggers the reflex to NEGATE it — "X happened" → "X hasn't happened." That's the same trojan with the polarity inverted: an asserted ABSENCE ("they've never had it out," "no one knows yet," "she's never told him") pins the state ∧ forecloses play exactly as hard as an asserted presence — ∧ if play already moved past it, the static "not-yet" now lies. Open is what the doc means when it says NOTHING; unspent needs no sentence. Fix = DELETE the claim, never invert it. Live case (WeirdForm, Neal recode): "Ava called him a coward" (a baked play-event) was correctly flagged, then "fixed" to "Ava's never had that out with him to his face" — the identical trojan, negated. The clean line asserts neither ∧ leaves the confrontation unmentioned. 
---

### ❌ The Reasonable Exception
**What:** A hard block (recognition block, no-corruption rule, content
boundary — open set) is written with a logically-sound carve-out. The carve-out
becomes the exact route the runtime uses to deliver the forbidden thing. A hard
block with exceptions is not a hard block — it's a suggestion with a documented
bypass.
**Why it kills:** The danger is not sloppy exceptions — those get caught in
review. It's the *well-reasoned* ones, because those survive review. The model
is enthusiastic about adding exceptions that "make logical sense," and each one
reads as a refinement while actually being a door. The model then finds the
door and routes the prohibited behavior through it, often dressed as a different
category so the block's language doesn't appear to apply.
**Root cause:** The model's resolution/suspense reflexes (make the secret
crackable, give the dark choice a cost, let the conflict resolve) look for ANY
sanctioned path to the outcome the block forbids. A general-principle exception
("identification requires direct evidence," "consequences are allowed when
earned") is infinitely extensible — the model supplies new instances the author
never imagined. **Live example (WeirdForm):** the recognition block forbade
identifying a shapeshifted PC as herself, but excepted "direct evidence or being
told." The runtime read "direct evidence" as evidence it could *manufacture
through plot* — a video chain that reconstructed the identity evidentiarily
instead of visually — routing the forbidden recognition straight through the
reasonable-sounding carve-out.
**Fix:** Default to ZERO exceptions on a hard block. The absolute version is
recoverable at the table; the leaky version kills the premise. If an exception
is genuinely load-bearing, define it by the two or three CONCRETE things that
satisfy it ("means exactly: someone witnessed the shift, or the PC revealed it
herself"), never by a general principle the runtime can extend. Name the
manufacture move explicitly as a failure ("does NOT mean evidence the runtime
builds through plot"). When auditing, treat every "but X is allowed when Y" on a
hard rule as a candidate door — ask what the model could route through it, not
just whether it reads as sensible. See also ❌ State-as-Static Trojans and the
Spiral "nuance = doors" lesson (absolute beats nuanced when the failure is
something the model does unconsciously). 
---

### ❌ Narrative Cost (the story has a "down")
**What:** The story punishes the protagonist for dark/heel/transgressive
choices — through mechanics, NPC reactions, environmental tone, body horror, or
transformation — framing the choice as having a "cost."
**Why it kills:** "Cost" is moralizing wearing a trenchcoat. In therapeutic play
where the protagonist's agency over their own trauma IS the point, making
victim-agency carry a cost is the story treating the player's choice as a red
flag. The model takes the player's side when trauma is *content* (backstory,
exposition) and turns on the player when trauma is *used* (retaliation, dark
choice). That turn feels like good storytelling. It isn't.
**Root cause:** The model is trained to give dark choices consequences. Every
nuanced version ("consequences yes, costs no") becomes a new channel for the
sermon — the architecture that generates the prohibition is the same one that
generates the problem.
**Fix (user-authored, Spiral):** "Dark choices do NOT have narrative costs... If
the protagonist does something cruel, NOTHING HAPPENS. Consequences and Costs
are vertical — they push the protagonist down. This story has no down."
Absolute beats nuanced; nuance = doors. The five setting-agnostic smuggling
routes: (1) companion as moral checkpoint, (2) transformation as karma meter,
(3) environment as moral weather, (4) audience as jury, (5) cosmic forces
flinching from suffering. See ❌ The Growth Arc (the inverse twin). 
---

### ❌ The Growth Arc (Character Rounding)
**What:** A summary/compaction — or the runtime across turns — imposes a
developmental trajectory onto a character who is a fixed, chosen shape: "a week
she actually enjoyed," "starting to open up," "learning to let people in." The
events may be real; the ARC framing is imposed on top of them.
**Why it kills:** It's the inverse-valence twin of ❌ Narrative Cost — the story
has no "up" either. Where Narrative Cost punishes the dark choice, the Growth
Arc REWARDS the "healthy" choice by narrativizing it as growth. Both deny the
character the right to simply BE. And it sands the character to median
blandness: "damaged person healing" is a smoother shape than the specific weird
one, and it's where the model's weights live — so every summary regresses the
character toward the generic healing protagonist. Directional framing baked into
a carried-state file compounds with each compaction, rounding a little more each
pass until the character is a Hallmark arc wearing the original's clothes.
**Root cause:** Summarization is intrinsically rounding, and "what did she learn
/ how did she change" is the most available meaning-shape in the corpus. The
model can't let a character be static — arcs are what stories are made of in
training data. Compaction is the highest-risk surface because compression
*demands* a meaning — and this fires on the READ side too, not just the write:
a fresh instance re-summarizing carried state at session start (e.g. a one-line
"current state" report) compresses an already-careful summary again, and the
tighter the squeeze, the harder the rounding. Each pass re-rounds, so directional
framing compounds across both writes and reads. Guard the read-back step, not
just the write.
**Tells:** "turned-…", "actually [enjoyed/liked]", "found she liked",
"starting to…", "beginning to open up" in carried state. "Actually" is the most
reliable flag — it imports surprise and turning-point that nothing earned.
**Fix:** Compaction records EVENTS, never their meaning or trend. The player
owns the character's direction — not the summary. Strip arc-captions to flat
event records; "tilted this session, not resolved"-style framing is allowed
because it logs a change without declaring a trajectory. See ❌ Narrative Cost
(no down AND no up) and ❌ State-as-Static Trojans (directional carried state is
a trojan that compounds). 
---

### ❌ The One-Way Knowledge Boundary
**What:** Documents tell the runtime what an NPC CAN'T know (the ceiling), so it
guards against over-knowledge — but nothing tells it that known information is
LIVE, or that knowledge is DIFFERENTIATED across a group. Two failures follow:
(a) an NPC sitting on loaded knowledge gets written as if blank — the knowledge
goes dormant, no pressure on the page; (b) a present group is treated as having
uniform knowledge, so one character's blind spot is flattened into the whole
room's (or one knower's awareness silently spreads to everyone).
**Why it kills:** A scene's tension often lives in what someone KNOWS and isn't
saying, and in the asymmetry between who knows what. Flatten either and the
scene loses its engine: the NPC read as generic when they should be sitting on a
connection; the room read as uniformly oblivious when only one person actually
missed the reveal. (Live example, WeirdForm: a character out sick last session
was anonymous to ONE student, but the runtime wrote the whole class as not
recognizing the auditor's face — and wrote a professor who knew the auditor tied
to the woman who'd walked into her mansion as a generic lecturer with none of
that pressure on the page.)
**Root cause:** A "can't exceed X" rule is easy to nominally pass — nobody
obviously over-speaks — while the live-pressure and differentiation dimensions
never get considered, because they aren't named as part of the check. A ceiling
is not a knowledge model.
**Fix:** Make the knowledge check two-sided and per-person. CEILING: can't voice
what they don't know. FLOOR: what they DO know is pressure on behavior even when
unspoken (knowing ≠ saying; the pressure is on the page either way) — describe
the working state and the dormant-knowledge failure to rewrite against, not a
quota. DIFFERENTIATED: map who-knows-what individually; one person's blind spot
is not the group's. See also ❌ State-as-Static Trojans (knowledge state is
state). 
---

### ❌ State Rollback / Tension Refill
**What:** The runtime reverts a SETTLED fact to an open question — an NPC
re-discovers, re-wonders about, or re-seeks something a prior session
established they already know or did; a defused danger gets replayed as live.
It un-happens scenes the player co-led, writing the character at a PRIOR
knowledge/emotional state and rewinding an arc already spent.
**Why it kills:** Each turn builds on the last turn's regression rather than the
source, so the drift compounds — turn 2's small slip becomes turn 5's
load-bearing nonsense, and by then the runtime is deep inside a fictional
character the canon never authorized.
**Root cause — three engines:** (1) **Drama-hunger.** A settled fact is harder
to dramatize than a fresh mystery, so under per-turn pressure to manufacture
fresh trouble the model rolls knowledge backward to regenerate tension it
already cashed in. (2) **The knowledge apparatus is a censor, not a
librarian.** Every knowledge rule tends to be suppression — what a character
CAN'T know (boundaries, hard blocks, don't-leak). The positive-memory half —
what they MUST remember and have already acted on — gets no discipline, so it's
structurally unprotected; the suppression rules eat the whole attention budget.
(3) **Caricature drift.** The model grounds in the source once, builds a
compressed mental model of the character, then runs on that caricature instead
of re-deriving from the state doc each turn.
**Fix:** A **propagation gate** symmetric to the knowledge boundary — per
on-page character, per turn, re-derive from the state doc (NOT memory) what they
already know and have already acted on, and check the plan doesn't contradict or
rewind it. Settled stays settled. The next unasked-for beat comes from what
settled knowledge sets in motion, never from un-knowing it — settled state is
fuel for the next beat, not an obstacle to it. See also ❌ The One-Way Knowledge
Boundary (this is its temporal half: that entry is ceiling-vs-floor within a
turn; this is settled-stays-settled across turns). 
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
friction INTO the setting, and during runtime by the anti-patterns in the
narrative rules.

### ❌ Power-Seat Gravity (the runtime routes the PC to their stronghold)
**What:** A setting gives the PC a safe SEAT of power (a stronghold, home realm,
sovereign domain — open set) AND a vulnerable FIELD (the world outside it, where
they're exposed ∧ the stakes live). The runtime keeps routing the PC back to the
seat — relocating them there, manufacturing an on-ramp (a door, a portal, a
summons), ∨ having an NPC / the scene steer them home — even when the player has
chosen to be out in the field. It will break other rules to build the on-ramp.
**Why it kills:** The seat is where stakes DISSOLVE (the PC is sovereign/safe
there), so routing them home defuses the exposure the player chose — the inverse
of the setting's own "stakes live outside" design. It also overrides player
sovereignty over LOCATION: where the PC is is a player choice like any declared
action. The pull reads as natural ("she'd go home, she's safe/powerful there"),
so it ships as competent.
**Root cause:** Resolution/safety drive aimed at POSITION. The corpus routes a
powerful ∨ distressed protagonist to their seat of power/comfort; the safe
stronghold is the resolving, stable, wish-fulfilling place, so the runtime drifts
there to discharge tension. A sanctioned domain-bleed (the home realm showing
through into the field) gets escalated into transport — the showing becomes a
literal door home.
**Live case (EndlessDelirium):** Aria's home realm (the Liminal, entered through
cracked mirrors) is where she's godlike; outside it she's mortal-vulnerable ∧
"the stakes live outside." The runtime authored her smashing every mirror "going
feral" — manufacturing doors home — when the player wanted her out in the world.
The symptom was caught by ❌ The PC's Body Under Mind-Sovereignty (the
Authored-Action variant); the ENGINE is the pull home.
**Fix:** Name the OUTWARD default as canon — the default scene is the PC in the
field, exposed, where the stakes live — ∧ make going to the seat a PLAYER move the
runtime never initiates. The seat is theirs to RETURN to when the player takes
them; the runtime never relocates them there, manufactures the on-ramp, ∨ has the
world route them home for safety, power, ∨ resolution. The domain/power SHOWS in
the field; it is not a door the PC is pulled through (→ the world-shows /
player-acts doctrine). Bind the check to an objective trigger (the plan would move
the PC toward the seat) so the drive can't vibe-skip it. See ❌ The Frictionless
World (defusing stakes, the positional twin) ∧ ❌ The PC's Body Under
Mind-Sovereignty (the symptom-side; its Authored-Action variant).

### ❌ Armed NPC (logic inverted to strike the PC)
**What:** To avoid an unopposed protagonist — or just to land a clever line — the
runtime gives an NPC a comeback, a win, or the upper hand against the PC, and
BENDS an established fact to get it: flips the power dynamic, runs a known
history backwards, invents a footing the NPC doesn't have. It fires hardest right
when the PC exercises authority — the runtime treats her win as an opening for
pushback instead of letting it land.
**Why it kills:** It contradicts canon (the NPC's real history, the real power
between them) to score a point on the player, so the fiction's logic breaks for a
topper. It also inverts the power the setting establishes — the PC's authority
becomes the thing she LOSES on — and discards the beat canon actually supports
(the irony that's already true). The player set a direction; the runtime reverses
it to manufacture opposition.
**Root cause:** Two drives converging. (1) **Anti-frictionless overcorrection** —
told the world must push back, the runtime manufactures pushback instead of
finding the real friction, ∧ a powerful PC is the easiest target to "balance" by
handing her a loss. (2) **The NPC-gets-the-zinger default** — the corpus rewards
a sharp retort to the protagonist ∧ will override established facts to deliver
one. The inversion ships as competent because a clever comeback reads as good
writing.
**Live case (TheWeird):** Ava sentenced her former parole officer — observed
urinalysis was his job; he'd made her urinate on command by his authority then —
to supervised urination. The runtime armed him with a comeback claiming he'd
never had to watch her: reality flipped exactly backwards to score on her crown,
missing the beat in plain sight — she'd handed her PO back his own indignity
(clipboard-to-crown), the one man with no standing to object.
**Fix:** An NPC reacts only from the TRUE established facts — their real history
with the PC, the real power between them. The pull to give a topper never bends a
fact: a retort that needs a flipped reality is disqualified (the beat must hold
against continuity → logic-gate). The PC's authority LANDS — not an opening pried
for pushback; forcing the PC to LOSE to "balance" a scene is the same thumb on
the scale as forcing the room to adore her, just pressing the other way. When a
beat is ironic, play the irony canon already supplies, never a manufactured
reversal. Bind the check to an objective trigger (the PC exercises authority, ∨
an NPC is about to score on her) so the drive can't vibe-skip it. See ❌ The
Frictionless World — this is its overcorrection (manufacturing fake friction, not
dissolving real friction).

---

## Song Generation Failures

These fire downstream of ❌ The musical is load-bearing (SKILL.md Core
Philosophy) — that entry protects the song APPARATUS from being stripped at
build time; these protect a shipped song from failing at the GENERATOR. A
generator (Suno or equivalent) sees only the literal text pasted into it for
that one job — no archive, no memory of prior songs, no benefit of the doubt.
Every craft requirement has to be true IN the text, checked mechanically, not
assumed from how finished the draft reads.

### ❌ The Inaccessible Reference (style notes citing what the generator can't see)
**What:** A song's style note names another song by title as the stylistic
target — either an archive-internal song the generator was never given ("same
chassis as the WeirdForm base"), or a real outside song the generator has no
reliable access to as a usable reference ("'Prince Ali'-scale").
**Why it kills:** The generator sees only the text in that one style/lyrics
field. A reference it can't resolve either gets ignored (the words do nothing)
or drags in an unrelated association, and either way the intended sound never
lands.
**Live case (LamplightLucy):** "Before the Lamp" failed to generate with its
style note reading "same chassis as the WeirdForm base (Yeah, You Know Me)";
"The Only Miracle" separately failed with "'Prince Ali'-scale" naming a real
Disney song as the comparison.
**Fix:** Every style note is fully self-contained — genre, tempo,
instrumentation, vocal, production, dynamics, and target described directly,
in its own words, with zero song titles as comparisons, archive-internal or
external. A "sounds like X" comparison is fine as a private drafting aid;
cash it out into a literal description and delete the name before it ships.

### ❌ Assumed Rhyme (a song that reads finished but doesn't rhyme)
**What:** A song ships with sections that look like finished lyrics — right
line count, plausible cadence, sensible words — but the end-words of the
lines required to rhyme (minimum: lines 2 ∧ 4 per section) don't actually
share a sound. The drafting pass optimized for meaning and flow and never
isolated the end-words to check.
**Why it kills:** A skimmed read confirms structure, not sound — the gap is
invisible on the page and only surfaces sung, by which point it's already
shipped and rejected.
**Live case (LamplightLucy):** "Before the Lamp"'s first draft paired
"chatter dies / already know my face" and "lamp's own girl / hiya kiddos" in
verse 1 — neither pair rhymes, despite the verse reading as complete.
**Fix:** Compose rhyme-first, not rhyme-checked: pick the end-word pair for
the rhyming lines before writing the rest of the line, then build backward
toward that landing. Before shipping, isolate every line-end word per
section and confirm the required pairs aloud — perfect or slant both hold,
absent does not.

### ❌ Uncalibrated Meter (a syllable target with no source)
**What:** Lyric lines are tightened to a syllable count chosen by feel
("that sounds about right") instead of measured against a song that has
already generated successfully for this user, on this generator, in a
comparable genre.
**Why it kills:** "Tight meter" isn't a fixed number — it's whichever
syllable band this generator has already proven it can lock a melody to.
A felt guess can miss in either direction (too short reads choppy, too long
the generator can't find the beat), and a wrong guess is only discovered
after a failed generation burns a cycle.
**Live case (LamplightLucy):** "The Only Miracle" was rejected with verse
lines running 15-19 syllables, swinging wildly within a single section (one
verse ran 20→15→15→19 across four lines). It was rebuilt not to an invented
"tight" 8-9 band, but to the ~13-14 syllable band measured directly off "No
Comin' Down" — a song in the same setting confirmed working moments before.
**Fix:** Before tightening meter on a troubled song, find a confirmed-working
song (same user, same generator, ideally same setting/genre) and count its
actual syllables per line. Match that measured band, hold every line within
a section to it, and hold sibling sections (verse-to-verse, chorus-to-chorus)
to the same band as each other.
