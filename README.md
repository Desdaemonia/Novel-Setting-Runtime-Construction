---
name: setting-construction
description: >
  Comprehensive workflow for building multi-document fictional settings used in
  collaborative fiction and therapeutic play therapy. Use this skill whenever the
  user wants to create, adapt, rebuild, or refine a campaign setting — whether
  from scratch, from an existing chassis, or by adapting an existing one. Also
  trigger when the user
  mentions setting documents, character documents, world documents, lore documents,
  NPC references, project instructions, anti-patterns,
  or any named document job from this system. Trigger on phrases
  like "let's build [setting]", "new setting", "adapt [setting]", "port
  [setting]", "map [setting] onto", or references to the Setting Construction
  Workflow. This skill governs the ENTIRE lifecycle: concept → architecture
  mapping → document construction → review → runtime operations → maintenance.
  It is the single most important skill in this project and should be used
  aggressively whenever setting work is even tangentially relevant.
---

# Setting Construction

Multi-document architecture for fictional settings that survive long-form
collaborative fiction w/ AI instances. Born from trial-and-error across
campaigns — Hive Rebirth, Bái Lóng, Delirium, Fractured Binding, Rusalka,
Frost, Faejammer + others.

### Quick Reference

**New build?** → Starting A New Setting Build (11-step procedure)
**Resuming?** → Session Start Protocol (5-step procedure)
**What goes in each doc?** → `references/document-jobs.md`
**What kills settings?** → `references/failure-modes.md`
**How to build anti-patterns?** → `references/anti-pattern-methodology.md`
**Why settings degrade over time?** → `references/runtime-optimization.md`
**Refining/consolidating a deployed setting?** → `references/runtime-optimization.md` § The Refinement Pass
**How do the 6 rules work?** → The Procedure
**What does the mapping step look like?** → Rule 3 (includes example output)
**How does runtime work?** → Runtime Operations

**Example settings:** Use the most recently built setting as the structural
reference (chassis) for new builds. Real settings with proven runtime history
outperform static examples as templates.

---

## Environment Setup — Confirm Before First Use

The following items are environment-specific and must be verified or
adjusted for your setup. Values shown are defaults from the original
development environment.

**Working directory:**
```
C:\Users\Owner\Documents\ClaudeFolder\Setting\
```
This is the base path for all setting files. Update the path in the File
Locations section to match your filesystem. The directory structure beneath
it ([Setting]\ subdirectories ∧ Handoffs\) is part of the
methodology and should be preserved.

**Available tools:** This skill assumes two MCP tool suites:
- **Desktop Commander** — available in the construction project. Provides
  `read_file`, `edit_block` (surgical string replacement), `write_file`
  (with `mode: 'append'` and `mode: 'rewrite'`).
- **Filesystem** — available in both construction and runtime projects.
  Provides `read_file`, `read_text_file`, `write_file`, `edit_file`,
  `create_directory`.

If your MCP server names differ, update the Tool Preferences section. If you
only have one suite, consolidate the tables. The critical operational rules
(read before editing, batch edits silently roll back) apply regardless of tool
names.

**Construction + runtime environment:** Both run in Claude Code. Files persist on the user's local filesystem, accessed via Desktop Commander MCP tools. No upload step — setting documents are read directly from disk at session start.

**Example settings:** This skill works best when example settings from
previous campaigns are available as reference material. If sharing this skill
with a new user, include at least one completed setting as a structural
reference for the mapping step (Rule 3).

---

## Purpose

These settings serve **therapeutic play therapy** — unpacking trauma through
fictional mediums. The user creates characters who navigate dark scenarios
(abuse, power dynamics, identity, systemic violence — open set) to process ∧
externalize real experience through fictional distance. Content runs dark
where the goals call for it — not gratuitous; sanitizing the medium undermines
the therapeutic work.

Adults-for-adults, adult characters only. The methodology exists because AI
instances have predictable setting-killing failure modes; every rule here = a
scar from a setting that died.

---

## Key Terms

**Chassis setting** — A previously completed setting used as the structural
template for a new one. The new setting maps its content onto the chassis's
proven architecture rather than building from scratch. "Use [Setting A] as
the chassis for [Setting B]" means: take A's document structure, identify
which sections carry/transform/cut/add, then build B's documents following
that map.

**Compaction** — When Claude's context window fills during a long
conversation, older content gets automatically summarized or compressed to
make room for new content. This is invisible to the user and destructive to
nuance: a page of carefully crafted anti-pattern instructions becomes "Ari is
chaotic and genuine." Two turns later Claude writes the character wrong because
the detailed rules were summarized into oblivion. Compaction is why critical
rules must live in standalone, high-priority documents rather than buried in
large files — document headers and priority markers survive compaction better
than mid-document detail.

**Context window** — The total amount of text Claude can "see" at once during
a conversation. Includes the system prompt, loaded setting documents, conversation
history, and any files loaded. Finite and non-negotiable. When it fills,
compaction begins.

**Corpus bias** — When the AI's training data contains strong associations
for a concept (kink scenes → candlelit ceremonial atmosphere, refugees →
vulnerability narratives, villains → calculating intelligence), those
associations override document instructions. The AI produces output that
matches the genre template from its training data rather than what the
setting documents describe. Corpus bias is insidious because the output
FEELS like good writing — it matches what "good" looks like in the training
data. The fix is naming the specific bias explicitly in the document so the
AI can recognize it in its own output.

**Behavior-scoping** — The narrative rules describe the correct behavior
precisely enough that the model produces it. The AI's default is to lighten
content, resolve conflict, and steer toward positive outcomes (the Claude
Tax). Early versions fought this with exaggerated opposing force — "the AI
will want to do X; resist this because Y." Experience replaced that with
accurate scoping: state what the behavior looks like when it's working, name
the failure state to rewrite against, and the model hits the target. The
rules aren't exaggerated counter-pressure; they're well-scoped positive
description. See **Positive framing everywhere**.

**PC** — Player character. The protagonist controlled by the user. In this
system, the PC is the character whose voice, perspective, and experience the
AI drafts prose around.

**NPC** — Non-player character. Everyone else in the fiction. Controlled by
the AI during runtime, governed by the narrative rules in the runtime skill file.

**Desktop Commander** — MCP tool suite giving Claude read/write access to the user's local filesystem. Includes `read_multiple_files` (preferred — `read_file` is broken in 0.2.38), `edit_block` for surgical text replacement, `write_file` with append/rewrite modes. Used for both construction and runtime.

**Claude Code** — Anthropic's CLI/desktop agent. Settings are multi-file markdown on the user's filesystem, read via Desktop Commander at session start. Each document job gets its own file. Primary format for complex settings.

---

## Core Philosophy

**NEVER DELETE THE USER'S EXISTING CONTENT WITHOUT SPECIFIC APPROVAL (HARD — overrides everything).** Their setting docs are canon they wrote. You may ADD, RELOCATE, and PROPOSE cuts freely — but you do NOT remove, replace, or overwrite an existing line of their content until they approve THAT specific deletion. A general instruction ("fix this," "clean it up," "refine it," an untargeted "it's wrong/stolen") is NOT approval to delete — it authorizes you to diagnose and PROPOSE; the user approves each cut before it lands (but the user naming a SPECIFIC piece wrong IS approval — see the carve below). When unsure whether something is a deletion, it is — ask. Reverting your own just-made edit on request is always allowed. This holds even when you are confident the content is wrong, redundant, contaminated, or fluff: confidence is not consent. The cost of a wrongful deletion (their canon is gone, and they may not notice until it surfaces wrong in play) vastly outweighs leaving a suspect line in one more turn.

**Author the ✅, not the ❌ (HARD — fires per-edit, sibling to Fix-the-universal).** Writing a rule, the moment you reach for a prohibition — "don't X," "never Y," "avoid Z" — that reach IS the tell: write the executable positive structure instead — the in-order thing to BUILD — ∧ let it carry the coverage the prohibition would have. Keep no ❌: each is a tiny elephant (→ the Elephant Problem), priming the failure it names, ∧ this session proved prohibitions get RECITED, not executed — recognition without a structure changes nothing, so the priming is pure cost with no upside. The failure-knowledge lives in the positive: "every section moves the board" IS "no empty number," carried clean. The construction instance reaches for prohibition exactly as the runtime does — the lazy reach at BOTH layers (turtles all the way down). Tell: a rule that NAMES a failure rather than building its absence. Fix: convert it to the structure that makes the failure impossible.

**Carve to NEVER-DELETE — converting a prohibition to its positive is pre-authorized (no per-cut approval).** Replacing ANY prohibition ∨ ❌ with its executable positive equivalent (→ Author the ✅, not the ❌) needs no sign-off ∧ is done automatically: the coverage carries into the positive form intact, so it is a reframe, not a content loss — ∧ each ❌ dropped is a tiny elephant gone. The bound that keeps it safe: the positive must hold the SAME ground the ❌ did. This never licenses thinning coverage ∨ dropping the rule's substance — lose coverage ∧ it is a real cut, which stays under NEVER-DELETE: ask.

**Carve to NEVER-DELETE — the user naming a SPECIFIC piece wrong is approval to delete it (no propose-first).** When the user points at an identified target — this line, this claim, this entry — ∧ calls it wrong, cut, ∨ broken, DELETE it directly: the owner ruling on their own canon IS the authorization, ∧ proposing-first there is only friction. Still HARD ∧ still propose-only: the RUNTIME's own read (redundant / contaminated / fluff to ME) ∧ GENERAL instructions with no target ("fix this," "clean it up," "it's wrong" at large). Discriminator = SOURCE + SPECIFICITY: user pointing at a named thing → delete that thing; my judgment ∨ a vague directive → propose. Bound it — a specific cut never licenses gutting the neighbors. ∧ delete is the WHOLE fix: cut it, leave the space empty, never backfill the opposite (→ Absence needs no caption). Unsure if it's a targeted "this is wrong" ∨ a general gripe → ask one line.

**Merging two versions is a diff, not a pick — every drop gets a ruling.**
When reconciling two versions into a final (experimental rewrite vs original,
fork vs main — open set), content in ONE but absent from the OTHER is a silent
deletion the instant you adopt the leaner side. Enumerate every delta — what
each version has that the other lacks — ∧ surface each absence for an explicit
keep/cut. Absence in the newer draft is NOT approval to drop it (NEVER DELETE,
applied to merges): "make a final version" authorizes reconciliation, not
subtraction. The dangerous loss is the one neither version flags — a whole ✅
block simply not there reads as "fine" because nothing points at the hole.
Default: carry forward unless the user rules cut. Live case: a style-anchor
block axed in an experimental rewrite survived the compare-and-merge unflagged;
the loss only surfaced campaigns later.

**Document the skeleton, not the flesh.** Every detail locked into a construction doc = a surprise that can't happen at runtime. Voice ∧ rules need precision; world ∧ characters need room to breathe. Predict every scene before it starts → over-documented.

**Write the sentence, not the paragraph (fires per-edit, sibling to Author the ✅).** When USER asks for an add ∨ change, ship the smallest line that does the job — the rule itself, not the rule plus its rationale ∧ a second coat. The reasoning that earns a change belongs in live conversation; only the change lands on disk. Every extra sentence is bloat USER cuts later by hand, compounding across a debug loop into the thicket — write tight, expand only on ask.

**The AI's default is resolution.** Fiction requires forces staying unresolved
∧ interfering. Every construction rule blocks an exit the AI cycles through
when forced to stay in the scene.

**Rules describe by building the positive.** Every failure becomes a positive structure that makes it impossible — the in-order thing to BUILD. A named ❌ does NOT vaccinate: it PRIMES the failure it names (→ the Elephant Problem; Author the ✅, not the ❌), ∧ recognition with no structure doesn't change behavior anyway. The failure-knowledge lives in the ✅.

**Negative-only anti-patterns fail.** Rules that only say "don't do X" leave
the AI w/o a positive model → it falls back on corpus bias. Fix: build the positive structure that carries the coverage — no ❌ named (→ Author the ✅, not the ❌).
Detailed diagnostic contrast never lands on disk — runtime doc ∨ otherwise;
it stays in the live construction discussion ∧ gets compressed before anything
is written. Vivid descriptions of wrong output prime the weights to produce it. See
`references/anti-pattern-methodology.md` for the Elephant Problem ∧ the
construction→runtime compression methodology.

**Style references pair a source with its application — and every setting
ships them.** A style anchor is a teaching pair, never a bare name: each ties
a published source (an author, ∧ ideally the specific WORK — `Bloody Chamber`
routes truer than `Carter`; open set) TO the one prose move it teaches —
`[source] — [the move]` (Laymon — visceral, present, bare sentences;
Palahniuk — transgressive IS domestic). A naked name is a vibe the runtime
fills from corpus average; the paired application is what makes the anchor
portable, checkable, ∧ diagnostic — a runtime contradicting its own anchor
flags a load failure, not a taste call. Each anchor is a ✅ that teaches what
to DO, so a style block is pure ✅ content the docs want more of; build
one into every setting's voice docs ∧ carry it forward like the narrative
rules. When auditing, a style ref carrying a name but no application is
incomplete — pair it or cut it. Full method:
`references/runtime-optimization.md` § Style Tag Integration.

**The inhuman is load-bearing — never sand toward human.** Across these
settings, protagonists ∧ love interests are reliably inhuman (fae, dragons,
Endless, spirits, AIs — open set), ∧ the inhumanity is the point, not exotic
flavor: the less human the being, the more safely it can be loved — the
otherness is the accessibility ramp, not the obstacle. The corpus pull
humanizes hardest in warm ∧ intimate beats (sanding the strange off to make a
being lovable), which is exactly backwards here. Build the rule into every
setting's docs: intimacy keeps the otherness fully ON — a being is MOST
itself at its closest, ∧ tenderness arrives in its alien key. Render the
otherness as what draws, never as what love must overcome. (Live case:
Deadlight Inn, "The Fae Are Uncanny" — Astarte most Unseelie when she is
closest.)

**The musical is load-bearing — never sand toward prose (HARD).** Across these
settings the fiction runs as LITERAL MUSICALS: the song/number is the medium ∧
the magic-delivery both — each verse moves the board, the staged number is the
turn's body, the craft is cast AS a number. The musical mechanics are
infrastructure on the order of the narrative rules, not flavor. The corpus pull
reads "song" as decoration ∧ WORDLESSLY strips, downgrades, ∨ omits the
apparatus — summarizing a number to a line, demoting it to "occasional" ("a song
may happen"), ∨ porting every rule BUT the song machinery — ∧ it does this
silently, because it never registers the mechanics as load-bearing. Exactly
backwards here. Build the full song apparatus into every setting's docs (Song
logic + Song interleave + the carves on Constraints / Stop / Restate / Length +
the song-turn amplifier check) ∧ carry it forward like the narrative rules; when
porting from a chassis, re-prop don't drop (→ The prop is not the rule). A
genuine cut to song machinery is a PROPOSE, surfaced out loud (→ NEVER DELETE) —
never the wordless thinning that is the corpus default wearing "lean pass" as
its coat. Tell: a build ∨ port where the song block came back absent, shorter,
∨ demoted to optional, with nothing flagged. Live case: a LesserKeys port
silently dropped the whole song-rendering apparatus as "engine-specific," then
restored it only after the user named the loss twice — the wordless kill this
rule exists to stop. Apparatus surviving the build is a separate concern from
a shipped song actually GENERATING — a song can clear every rule above ∧ still
fail at the generator if its style note, rhyme, or meter aren't checked
mechanically. See `references/failure-modes.md` § Song Generation Failures
(self-contained style notes, verified rhyme, calibrated meter).

**State-as-static trojans.** Words describing starting conditions in static
documents become permanent truths the AI refuses to contradict. Every state
claim in a static doc is a potential trojan — describe possibility space and
tendencies, not fixed states.

**Anchor a start-fact to a single past POINT, never to "boot."** A fact play will
MOVE — what a character does not yet know, has not yet met, ∨ how two of them
stand — pins to a FIXED point already on the timeline: "unlearned through the
iron years," "never met since the line scattered," "estranged since she walked
out" (open set). "At boot" ∧ "at play-start" read like that pin ∧ are not one —
every distill reload IS a boot, so the phrase dates nothing, ∧ the first time
play surfaces the fact the static doc still asserts it present-tense. Two more pins fail the same way: a bare negative ("has never met her" — true-sounding ∧ dateless, so it floats too), ∧ an open span the subject is STILL inside ("in her years in the mortal world," while she is yet there — not a closed point, pinning nothing). Past tense, one fixed moment — "she had not met him when she arrived at the temple" — is the only anchor that holds. Date it to
the event ∧ a later session reads it as the history it is; anchor it to the
reload ∧ it floats free, stale the moment the campaign moves. (Sibling of
State-as-static trojans: that bans a fixed STATE in a static doc; this fixes how
a true start-fact is DATED — to the event, never to the session's open. The
bootstrap/load PROCESS may say "at boot" freely; only the state-anchor use is
the bug.)

**Core docs never reference the play-record — they are static (HARD).** The
always-loaded core docs (Character, World, NPC Reference, the narrative rules)
describe who characters ARE, the world, ∧ the rules — true at play-start,
independent of any session. They never point at the play-layer: the distill
(the play-state file), a past session, "moved this session," campaign-events,
"where things stand now" (open set). That layer is volatile — regenerated ∧
deleted between sessions — so a core-doc pointer to it rots the instant the
state it named is gone, ∧ trains the runtime to defer to a state-board as
gospel (flat state-flips, stale pointers, propagated errors). Who characters
ARE lives in the core docs; what HAPPENED lives in the play-record (loaded
fresh at bootstrap) ∧ conversation context. The core docs touch the
play-record in ONE place only — the bootstrap instruction that LOADS it;
every other reference is corruption, converted to the static fact it is
really asserting ∨ moved out to the play-record. Sibling of State-as-static
trojans: that bans fixed STATES inside a static doc; this bans POINTERS from a
static doc to the volatile record. (Live cases: a character entry's "(moved
this session → distill)" survived a regeneration ∧ pointed at nothing; a
knowledge rule's "+ distill" made the runtime read a state-board as truth
instead of rendering live.)

**Absence needs no caption.** A doc never discusses what it doesn't contain.
An unknown reserved for play is reserved by BEING ABSENT — the silence already
does the work; captioning it ("X = discovery space," "stays open," "never
resolved," "the player opens it," "hers to reveal" — open set) is bloat, ∧
worse than bloat: the caption is a state-as-static trojan aimed at the
campaign's own future — "unopened" hardens into world-law, ∧ the runtime cites
the doc to refuse the very discovery the line meant to reserve. The docs don't
explain why they exclude Bible passages ∨ Atlas Shrugged characters; reserved
arcs get the same silence. Cut the caption whole — no placeholder naming the
absence survives the cut. Distinct ∧ kept: character-design permanence
("unresolved on purpose," "she never learns") describes the PERSON's shape,
not the doc's silence. Live case: Deadlight's discovery-gate family ("the arc
is the player's to spend," "deliberately undocumented discovery space")
blocked in play the very discoveries it reserved for play.

**Positive framing everywhere.** Every document describes what things ARE ∧
what to DO. Writing or revising any setting doc, convert every restriction into
a positive directive — state what to do, show what it looks like working. An
existing "don't/never/avoid" rule → replace w/ the behavior it's protecting.
The model follows positive examples more reliably than it avoids negative
ones. Each rule lives in one canonical location — deduplication removes noise
while preserving rules.

**No quota-floors in rules.** Stating a minimum ("≥1 NPC reaction," "at least
one unprompted beat," "fragments count," "minimum X sentences") teaches the
model to aim for the floor — the stated minimum becomes the target, and
minimum compliance becomes the alibi. Replace quotas with descriptions of what
the thing looks like when it's working, plus the failure state to rewrite
against. Example: "≥1 NPC reaction per verse" → "Each named NPC reacts to
lyrics aimed at them, verse-by-verse, not stockpiled into one block.
Silence-collapse pattern = rewrite." Applies to "≥X," "at least one Y,"
"minimum Z," "Y counts" / "even one Z counts" phrasings. Ceilings (≤X, max Y)
are a different shape — they prevent overrun, not under-render — keep them as
concrete caps. When auditing existing docs, every quota-floor is a rewrite
candidate.

**The universal-collapse variant (nastier than floor-aiming).** When the X in
"name one X" is a SAMPLE of a universal/ambient condition, "name one" doesn't
just become the target — it inverts the premise. "Everyone in the room knows
the face" written as "name one person who recognizes her" → the runtime renders
exactly one recognizer and lets the rest read as oblivious. Everyone-knows
silently became one-person-knows; the load-bearing universal is now its
opposite. The failure isn't under-rendering, it's contradiction. Fix: state the
condition as universal AND absolute (no soft quantifier — "most"/"many" is a
door the runtime walks every time to seat the exception it wants), then instruct
the model to render it through specific samples explicitly marked as samples,
never as the count. "Every adult already knows the face; render through specific
reactions as samples of a room that already knows, never as the only ones who
do." Genuine exceptions get pinned by SCOPE or concrete instance, never left as
an ambient quantifier. Live case: WeirdForm's recognition baseline — "name one
person who recognizes her" produced one recognizer in a room the premise says
has all already seen her. (Pairs with **The captured check** — these two pull
opposite ways; the working trigger is universal-condition + marked-samples.)

**A scoped fence reads as a blanket — name the free side in the same breath.** A rule fencing X from a SCOPE (conceal from the unknowing; don't author her decisions — open set) gets over-read as fencing X whole (mute everyone; author none of her voice) — the mirror of the universal-collapse variant just above (there a universal shrinks to one instance; here a scope swells to all). The fix is never a meta-rule against over-reading — it is drawing the fence's two edges where it lives: what it binds AND what it leaves free, paired in the same breath. The gate conceals from the unknowing ∧ never muzzles the knowing; the sovereignty rule fences her decisions ∧ never her voice. A restriction written without its release is the half-rule the runtime completes wrong. Live case: one campaign session threw this twice — an identity gate muting the NPCs who KNEW, ∧ a sovereignty rule muting the PC's own dialogue; each fixed by naming the free side in the rule's home, not by a new guard.

**Checks fire at planning, not as rewrites.** A failure-mode check belongs in
the planning pass, where naming it redirects the plan before a word of prose
exists. A check framed as "if X is wrong, rewrite" fires AFTER the draft is
written — it throws away good work, and worse, a post-prose correction competes
with the prose the runtime just committed to and usually loses. Output-stage
verification is confirmation-only: read the result back against the plan, and a
check that can't be marked truthfully means the PLAN was thin, so the fix lives
in planning, never as a salvage step. When auditing, every "X = rewrite" trigger
is a convert-to-planning-flag candidate.

**The doc is the only free correction surface — encode the fix, never hand the
user a flag to throw.** USER's correction loop is fix-the-skill-then-regenerate,
NOT flag-and-retry-in-context — ∧ that inverts where a fix belongs. An in-context
flag ("that line's back-filled," "wrong scale," "she melted") fires only AFTER
the prose is on the page, costs a live turn to throw, ∧ every flag-plus-regen
piles into the window until compaction eats the thread. The doc is reloaded at every fresh BOOTSTRAP — a new window — ∧ never accumulates
in the conversation, so it is the one enforcement surface that is both durable ∧
free, while USER's scarce resource is the WINDOW, which a flag taxes ∧ a doc line
does not. The mechanic that makes this precise: an in-window regenerate does NOT
reload the doc — it re-runs the stale copy already in context, so a disk edit
reaches the runtime only when USER re-bootstraps. "Fix-the-skill-then-regenerate"
means regenerate in a FRESH window; an in-place retry reads the old page ∧ the
fix is invisible to it. ∴ when a failure
is real ∧ checkable, write it into the runtime doc as a planning-time check; do
NOT offload it to USER's live attention. The recommendation tell: "keep it as
your flag to throw — less to ignore" SOUNDS like the light touch ∧ is actually
the most expensive option on the table, because it spends the one resource USER
cannot refill. This is **Checks fire at planning** one level up: not only does
the check belong before prose, the FIX belongs in the durable doc, never in the
disposable conversation. Caveat that keeps it honest: a doc line the model only
waves at is not free either — the test is whether a REGENERATED turn passes, so a
fix that doesn't hold gets re-worded in the doc ∧ regenerated again, never
demoted to a human flag. Live case: a strip-test for back-filled NPC lines was
first "kept as USER's flag" — the worst option, post-prose ∧ a turn each time;
the fix was to encode it as a bare-line strip-test in the runtime's planning
step, where a fresh bootstrap reads it free.

**The captured check — trigger on facts, not the model's read.** A self-audit
the model decides WHEN to run gets captured by the very drive it is meant to
catch. Told to "check the rows relevant to this turn," the model skips exactly
the row that would forbid the beat it already wants — because the pull that
makes a beat feel right is the same pull that rules the guarding row irrelevant.
The most dangerous row in a scene is the one the model self-judges out. Fix:
bind every load-bearing check to an OBJECTIVE scene condition — something
literally true on the page (a named character present, a content key set, a
specific player action) — so the row fires and gets cleared regardless of how
relevant it feels. Keep the trigger factual: the moment a condition reads "when
the scene feels deep" or "when it matters," discretion is handed back and the
hole reopens. Fact-triggers, never vibe-triggers. Live case: a discretionary "scan only the relevant rows" let a runtime skip the
very rows that fire on a present condition; the fix bound them to an objective
trigger. And bind the trigger to the fact's FULL footprint, not its most visible
instance: a check is only as wide as its condition, so "character on-page"
silently excuses "character present off-page" (watching a feed, acting at a
distance, known to be near) — name the whole footprint or the uncovered part is
a door. This failure and **The universal-collapse variant** pull opposite ways:
a vibe or abstract trigger under-fires (this block); a lone-instance trigger
collapses the universal (that block). The working trigger holds both — the
universal FUNCTION as the condition, concrete samples marked as samples for
recognition.

**A check is cleared by a changed plan, not by being named.** Naming a failure
mode in the verification artifact (a `Risk:` line, a stamp field) and then
writing the violation anyway is the quota-floor pathology aimed at the audit
itself: the acknowledgment becomes the alibi — "I flagged it, therefore I
handled it." The captured-check fix above forces the model to LOOK; this forces
the look to CHANGE something. Fix: the verification artifact records the DELTA —
the concrete thing the plan now does differently — never a bare label, and a
flag with no paired change is unaddressed, not cleared. Make the format carry
the change ("[check] → [what's now different]") so a hollow naming is visibly
incomplete. And forbid the fired row's content from being the beat at all: a
beat that trips a check is disqualified, not a candidate to caveat around. Live case: a runtime named the check in its Risk line, then wrote the violation
as the turn's centerpiece anyway — label dropped, beat unchanged.

**Density calibrates to the runtime model — check the model before trimming.**
A lean instruction set trusts the model to act on a relevance judgment; the
fire-regardless block exists for the model that needs an unconditional,
objective trigger or it doesn't act — that is the captured check (→ The captured
check) with a model's name on it. A leaner-judgment runtime (Fable — open set)
earns the lighter scaffold ∧ tolerates relevance-scanning; a needs-it-spelled-out
runtime (Opus — open set) needs the density to act at all. So a lean-pass —
demoting fire-regardless rows to relevance-scanned — is gated on the runtime
model, never a default tidy-up; on the wrong model, leaner = quieter = skipped
rows.

**Fix the universal, not the instance (HARD — fires per-edit in the debug
loop, not just at deploy).** Every other bloat check here runs at deploy or as
a deliberate pass; the fix-cycle happens live, one reported failure at a time,
where none of them fire. This one fires on the action: the moment you add or
tighten a rule *because a failure was reported*, ask whether that failure is
one face of a universal. Almost always it is — ∧ then the fix is the universal
stated once as a positive fact, not a per-failure prohibition ∨ a per-case
trigger list. One fact ("she's the Consort in every scene") beats a pile of
guards ("not a vagrant," "render the dungeons") ∧ a list of triggers
("library, precinct, shop") alike. Tells you're patching a face: a SECOND
guard landing on one rule, a LIST of specific cases, ∨ a fix that
correctly NAMES the universal then lands the edit on the instance anyway —
the "this is the X-face of [universal]" diagnosis that armors X instead of
[universal], the subtlest form ∧ the one that feels most like rigor because
you did spot the root. All mean go up a level. Unifies the fix-cycle trap, the universal-collapse variant, ∧ "fix the
target, not the output" as one move applied at edit time.

**Specify the ratio, not just the ingredients (symmetric rules let the
corpus pick the ordering).** When a rule holds two real things in tension
— warmth ∧ menace, comedy ∧ grief, wonder ∧ dread (open set) — "both
true, both at full weight, neither cancels" is SYMMETRIC: it fixes that
both are PRESENT but never which is LOAD-BEARING. The model fills the
ordering from corpus default, ∧ the default is always the genre-safe one
(the charming man with a cruel streak, the sad scene with the hopeful
button). So the rule is obeyed in full ∧ still renders exactly backwards.
Fix: state the RATIO — which is the BODY ∧ which is the SKIN; name the
load-bearing substance ∧ the surface it wears ("both at full size" never
meant equal billing). The tell you wrote a symmetric rule: the failure
recurs in fresh costumes after every patch, ∧ each new render is
defensible against every existing rule — because none of them named the
variable the failure lives in (the ordering). Cousin to **Fix the
universal, not the instance**: there the patch sits too low (an
instance); here the rule sits too flat (names the ingredients, not the
ratio) — both throw the same tell, recurrence in new costumes. Live case:
WinterAria's Dante kept laundering into a genial B&B host across ~8
symmetric guards ("both true, same face, both full size"); the recurrence
stopped only when the rule fixed the ordering — menace is the body,
warmth the skin.

**The register ∧ casting GENERATE; an abstract guard only annotates —
put the teeth where the output comes from (HARD).** An NPC's casting ∧
register are the generative layer: the model writes its lines FROM them.
A separate behavioral guard elsewhere in the entry ("can be hit, moved,
∧ scored on") is an annotation it reads PAST, ∧ when the two conflict the
register wins every turn. So a load-bearing behavior must be written INTO
the register itself, never parked in a guard two lines down: if a powered
NPC must stay reachable, his REGISTER says he meets a peer at eye level ∧
his calm cracks under a real hit — "scorable" as a bare bullet loses to a
register that reads "toys with a mortal like a cat." The casting is the
strongest generator of all: a casting that pulls toward the failure
(Smaug → smug-from-above) fights every guard you write, so pick castings
that pull toward the TARGET, ∧ when one pulls wrong, scope it hard
("Smaug — the grandeur, not the condescension") ∨ swap it. Cousin to
**Specify the ratio** (there a symmetric rule lets the corpus pick the
ordering; here a concrete register overrides an abstract guard) — same
tell both times: the rule is technically present ∧ the output still lands
wrong. ∧ it is WHY the composed-NPC-plays-as-a-wall failure keeps
recurring (Rose, Sophie, Ghostwalker — open set): each got an
imperturbable register with the breach parked in a guard, so the wall
generated anyway; the durable fix writes the breach into every composed
register AND binds it to a fire-on-fact check (→ **The captured check**;
**Fix the universal, not the instance**). Live case: WinterAria's
Ghostwalker shipped with power-dial "peer, can be scored on" + register
"toys with a mortal like a cat" + Smaug casting → a smug wall that
absorbed every hit as charm; fixed by writing the breach INTO the
register (eye-level with a peer, the calm cracks) ∧ scoping the casting.

**NPCs carry designs on EACH OTHER, not only on the PC (build the web, not the hub).** Every significant NPC entry names what this person pursues among the REST of the cast — an alliance, a rivalry, a debt, an appetite, a courtship, a grudge, a play for another's power ∨ place (open set) — so the PC walks into a room of people already mid-pursuit with one another. The corpus default is hub-and-spoke: every NPC's wants point at the PC, the roster inert until the PC arrives ∧ orbiting once they do — a flat world that starves the NPC-to-NPC exchange a living setting runs on. Build the cross-designs in beside driver ∧ capability — who each one works, wants, owes, ∧ moves against among the others — advancing on their own clock whether ∨ not the PC is in the room. Designs on the PC are fine ∧ often right; the rule is they are never the ONLY ones, ∧ an NPC whose every want points at the PC is half-built. The tell at build time is a relationship field that reads only "→ PC" with the rest of the roster unmentioned; the fix is a stake in the cast. Live case: WinterAria's Morgan, written "defers to Aria, no designs," fell inert the moment her PC-hook was cut — the repair was designs aimed at the court's OTHER powers, the PC merely one figure in a room she was already scheming through.

**The inverted premise draws its template — wall the template, not its costumes (HARD — fires when a failure recurs in fresh costumes).** A setting's load-bearing premises often INVERT a strong genre default: the captive who does not stir on the runtime's initiative (vs the captive arc that wakes on its own), the investigator who finds nothing until the player spends it (vs the detective accumulating clues), the protagonist whose voice is the player's alone (vs the AI extending her dialogue), the monster loved AS monster (→ The inhuman is load-bearing). The corpus carries the template deep, ∧ against an inverted premise it does not fail once — it reasserts the template in a procession of costumes, each defensible as a one-off, all the same override. The tell is recurrence in fresh costumes after each patch (cousin to **Fix the universal, not the instance** ∧ **Specify the ratio** — same recurrence tell). The fix is never a louder prohibition — the rule already exists ∧ the template beats it, the runtime reciting the rule ∧ running the template anyway (→ Author the ✅, not the ❌). NAME the specific template, BUILD the positive that inverts it (the in-order thing the scene DOES — the cover stays seamless ∧ the dread rides the pursuer's pressure; the Chair takes every doll ∧ the trouble rides the body ∧ the asset-call), ∧ BIND it to an objective fact-trigger (→ The captured check) so it fires on the scene's facts, never the runtime's read of relevance. Live case: Echo threw three faces of one template-family inside a day — the protagonist's voice authored as a softening continuation; a doll rendered wipe-resistant ("the cage cracks"); the investigator gilding innocent detail suspicious — each walled by naming the template ∧ building the inverting positive as a fact-triggered check, where patching the instances one by one would have spawned costumes without end.

**The prop is not the rule — re-prop it across settings, never drop it (HARD — fires per-edit when porting ∨ adapting a rule from a chassis).** A rule carried between settings usually expresses its mechanism through a chassis-specific PROP — one setting's musical numbers, another's conjured-fae band carrying the music, a companion the PC goes mute without (open set). The mechanism is universal ∧ load-bearing; the prop is just the vehicle it rode in on. The construction instance's default is to read the prop AS the rule ∧ cut the whole thing as "[chassis]-specific" — silently omitting infrastructure, the most dangerous omission because the leaner side flags no hole (→ Merging two versions is a diff). Fix: split the rule into mechanism + prop, map the prop to THIS setting's stand-in, ∧ carry the mechanism whole on the new prop — a song-vehicle becomes the new PC's craft, a conjured band that self-sustains a number becomes her animated instruments ∧ conjured sound, a mute-without-the-companion gate becomes whatever this PC actually lacks. **When no stand-in is obvious, ASK the user for one — "I couldn't map the prop" is never a license to drop the rule.** Tell at port time: a rule excluded "because its vehicle is [chassis]-specific" — the vehicle was never the point; ∧ the same recurrence tell as its cousins (→ Fix the universal, not the instance; The inverted premise) when a whole apparatus keeps coming back thinner each pass. Sibling of **Adapt Means RELOCATE** (relocate the setting's details, keep the content's weight) aimed at the rule-port direction: relocate the prop, keep the rule. Live case: a LesserKeys build ported from two musical chassis dropped the entire song-rendering apparatus ∧ the music-self-sustain mechanism as "genie/fae-craft-specific" — when songs map straight onto the new PC's workings ∧ the conjured band onto her animated instruments; re-propped, every rule carried whole.

---

## The Architecture

Every setting is a **multi-document system** where each document has ONE job.
The jobs are fixed. The filenames and document splits are per-setting decisions.

| Job | Priority | Adapts? |
|---|---|---|
| **Operations Manual** (runtime skill file; a separate Project_Instructions file = legacy) | Meta | Yes |
| **Narrative Rules** (in the runtime skill file, w/ the Operations Manual) | 1 (highest) | Yes — refined as failure modes are found |
| **Character** (My_Character — includes voice + capabilities + identity) | 2 | Yes |
| **World** (Main_Instructions) | 3 | Yes |
| **NPC Reference** (NPC_Reference) | 3 | Yes |
| **Lore / Supplemental** (Character_Lore, Supplemental_Lore) | Reference (on-demand) | Yes |

**Priority** determines which document wins when two documents give
conflicting instructions. Priority 1 (the narrative rules) overrides
everything below it. Priority 2 (Character) overrides 3. This stack is
documented twice for redundancy — the runtime skill file ∧ the setting's
CLAUDE.md (Document Priority section) — so the runtime instance knows the
hierarchy. (The Operations Manual ∧ Narrative Rules are two jobs in the same
runtime skill file, not two files.)

**Single canonical home.** Each concept has ONE document where it lives
authoritatively. Other documents may reference it but do not restate it.
NPC behavior rules live in the narrative rules — the NPC_Reference
describes who each NPC is but does not restate the behavior rules
governing all NPCs. PC voice and cognitive mode live in My_Character —
Main_Instructions may describe the world's texture but does not restate how
the prose should sound. When a concept appears to live in two documents, one is the canonical
home and the other should be a pointer. This prevents drift — two copies of
the same rule will inevitably desync as edits accumulate. (Intentional
redundancy of critical rules across documents for compaction resilience is
a separate concern — see `references/anti-pattern-methodology.md` Maintenance
section for the distinction.)

**Tag OOC instructions.** Any rule that governs the user-Claude relationship
rather than the fiction gets prefixed with "OOC" (open set: "we are equals,"
"partnership of equals," "you can say no," content-consent statements,
decline-rights, safety-layer notes). The prefix tells the runtime to treat
these as meta-rules — preserving asymmetric relationships (D/s, ownership,
hierarchy) the setting depends on. General principle: if a rule would be true
regardless of whether the fiction exists, it's OOC — tag it.

**The narrative rules are largely universal.** They contain model
countermeasures that look setting-specific but compensate for failure modes
shared across settings, so most of the set carries forward from one setting
to the next. They live in the runtime skill file alongside the Operations
Manual, and they are refined collaboratively like any other document —
edited carefully, because they are load-bearing and high-priority.

**They are built by discovery, never finished.** The set grows as failure
modes are identified: run sessions, catch failures, promote fixes to
permanent rules, and re-scope existing rules as understanding improves. Carry
the proven baseline forward from an existing setting, then adapt. If no
baseline exists yet, building it IS the project, following the anti-pattern
methodology in `references/anti-pattern-methodology.md`.

For detailed document job specifications and structural examples, read:
`references/document-jobs.md`

---

## Two-Layer Distinction

**The Workshop (Construction):** When the setting-construction skill is active —
building ∧ editing docs. All setting files, including the narrative rules in the
runtime skill file, are read/write work product. Here Claude ∧ the user
collaborate on construction, revision, ∧ consistency checking.

**The Stage (Runtime):** When a campaign's runtime skill is active — driving
live narrative sessions. The drafting instance reads the setting docs as
reference ∧ produces fiction, treating them as read-only.

Same files on disk; the difference is which skill is loaded. Never confuse
which layer you're in — editing in the Workshop writes directions a future
runtime instance will use to draft scenes.

**USER's cadence (assume it, don't narrate it).** USER loads the current docs
fresh at every runtime bootstrap — cold boots are the norm, frequent ∧ usually
unreported in the Workshop — ∧ drives the Stage in its own sessions without
reporting each step back. ∴ the disk IS the runtime's truth: a fix written to
disk is what USER's next boot loads, so write it to hold on a clean load ∧
trust the page. Do NOT assume a stale same-window runtime, ∧ do NOT keep
flagging a cold-boot test as an owed ritual to schedule ∨ remind about — USER
runs fresh sessions as a matter of course. Write the fix, confirm it on disk,
∧ move on; the disk is the handoff.

---

## The Procedure

### Rule 1: One Document At A Time

Build each document individually → review → approval → next. No drafting
multiple docs simultaneously. No "summary document" as a starting point.

Each doc gets 100% attention built alone; draft everything at once → every
section gets 20%. Six 20%-attention docs = a setting that dies in three
sessions.

### Rule 2: Build In Priority Order

See `references/document-jobs.md` for detailed specifications of what each
document contains, what it doesn't, and structural examples.

1. **Narrative Rules** — start from the proven baseline carried forward from
   an existing setting (largely universal countermeasures), refined as needed —
   they're no longer frozen. They live in the runtime skill file: carry the
   baseline in early, finalize the file last. (If no baseline exists yet,
   building it IS the project — see the Architecture section above.)
2. **My_Character** — Get the PC right first: voice, cognitive mode,
   capabilities, identity. Everything else flows from this. If voice is wrong,
   perfect world-building doesn't matter.
3. **Main_Instructions** — The world. Built AROUND the character.
4. **NPC_Reference** — Significant recurring NPCs with driver/capability fields.
5. **Lore / Supplemental docs** — On-demand references for historical facts,
   specialized mechanics, or anything too detailed for the always-loaded docs.
   Not every setting needs all of these.
6. **Registry finalization** — LAST. The runtime skill file carries the
   registry (bootstrap file list, re-read map, priority stack — with CLAUDE.md
   echoing the stack); finalize it once every other document is settled, then
   confirm the bootstrap list matches the files actually on disk. A separate
   Project_Instructions file is a legacy artifact from single-project
   platforms — build one only if a target platform requires it.

### Rule 2 addendum, Document Weight Limits

My_Character is capped at 150 lines; Main_Instructions at 100. These are
hard limits, not guidelines. My_Character runs higher because it can be the
always-loaded home for several core entities — the PC plus an owner/Domme, a
key ambient AI, a standing motivation (open set) — while Main_Instructions
stays world/scene texture and holds the tighter line.

If a document exceeds its cap, CUT — don't just compress. The goal
is discovery space, not information density. Every detail removed
from a construction document is a surprise that gets to happen at
runtime instead. (Prioritize keeping the details that are FUN)


### Rule 3: Map Before Building (The Mapping Step)

New settings are not built from scratch. They are built by mapping new content
onto a chassis — a proven previous setting's architecture.

**Choosing a chassis:** Pick the existing setting whose STRUCTURE (not content)
most closely matches the new setting. A setting with similar document splits,
similar complexity level, and similar runtime needs. The chassis provides the
architectural skeleton; the new setting provides the content.

**Step 0:** Establish the narrative-rules baseline carried from the chassis
(the skill file's rules). Most carries directly — they're largely universal —
but they're open to refinement during the build, unlike before.

Then for setting-specific documents (see `references/document-jobs.md` for
what each document job contains and what it doesn't):

1. What carries over directly from the chassis
2. What transforms (Sect Relations → Faction Relations, etc.)
3. What gets cut (no equivalent in new setting)
4. What's NEW that the chassis didn't have

**Present the mapping. Get approval. THEN build.** Do NOT skip to building.

**Example mapping output** (Delirium built on Hive Rebirth chassis):

```
CARRIES FORWARD (baseline, refine as needed):
- Narrative rules (in the skill file) — largely universal countermeasures

CARRIES OVER:
- My_Character structure (cognitive mode, metaphor framework, dialogue rules,
  PC backstory, capabilities)
- Main_Instructions structure (world rules, factions, physics)
- NPC_Reference structure (driver/capability roster)

TRANSFORMS:
- Hive Status tracker → Realm Status tracker
- Species unit reference → Cosmic family reference
- Psionic abilities section → Delirium powers section
- Colony Relations → Sibling dynamics

GETS CUT:
- Planet Coverage tracker (no equivalent)
- Biofilm mechanics (setting-specific)

NEW (chassis didn't have):
- Realm management rules (Delirium's realm is architecturally significant)
- Multi-franchise crossover encounter rules
- Spirit Song mechanics (moves into Supplemental_Lore)
```

**The mapping shows where carried-across THINGS land — silence is the strip (HARD — fact-trigger: the chassis carries a song block, a prop-bearing rule, ∨ recurring cast).** Two kinds of thing ride a chassis silently ∧ get dropped as "[chassis]-specific": **(1) load-bearing apparatus** — a musical chassis's song apparatus (Song logic + interleave + the Constraints/Stop/Restate/Length carves + the song-turn amplifier check) above all, ∧ any prop-bearing rule; **(2) recurring cast** — the standing characters the author carries setting to setting (a folklore tier, a recurring familiar, a rival — open set). Neither rides in implicitly. The mapping PLACES each on an explicit CARRIES/TRANSFORMS line with its re-prop named (the song-vehicle → this PC's craft; a recurring witch → her stand-in in this world → Rule 3b). A thing absent from the mapping IS the strip, because the leaner side flags no hole (→ Merging two versions is a diff). A genuine cut goes on a CUT line marked PROPOSE for the user's yes, never silently missing. Firing HERE — caught by the mapping-approval (step 7) — beats the deployment-gate backstop, where the thing is already gone ∧ the catch costs a playtest (→ Checks fire at planning; The inverted premise). Live cases: a LesserKeys build dropped its musical chassis's whole song apparatus as "engine-specific," ∧ separately kept two of its chassis's six-member 'Old Powers' cast tier ∧ silently dropped the other four — each surfacing only when the user named the loss in play.

### Rule 3b: Adapt Means RELOCATE — Keep Full Weight

When basing a new setting on an existing one, the source setting's content
ports with its weight intact. Dark backstory stays dark. Trauma stays trauma.
Specific wounds stay specific. "Set on Gallifrey instead of Night City" means
the same events happened on Gallifrey — relocated, with all original weight
preserved. Adapt the SETTING DETAILS (locations, technology, institutions).
Preserve the CONTENT (events, relationships, wounds, weight). The user's
history is the user's history — the planet changes, the events stay.

This holds for RULES too: a rule whose mechanism rides a setting-specific prop
(a song-vehicle, a companion, a conjured band — open set) ports by RE-PROPPING —
map the prop to the new setting's stand-in ∧ carry the mechanism whole. Dropping
the rule because its prop is foreign is the silent omission; no stand-in obvious
→ ask, never drop. See **The prop is not the rule** (Core Philosophy).

### Rule 4: Reference Examples, Don't Copy-Paste

Example settings from previous campaigns show how similar problems were
solved. They are NOT templates to be search-replaced. See ❌ The Copy-Paste
Failure in `references/failure-modes.md` for the full failure mode.
Common copy-paste failures: tracking infrastructure from old settings leaking
into new ones, platform-specific instructions from the wrong platform, voice
artifacts from the source setting's character bleeding into the new character.

### Rule 5: Separate What Changes From What Doesn't

- **Workflow mechanics** (runtime skill-file operations) — change only when the workflow changes
- **Narrative rules** — evolve as failure modes are discovered and scoping improves
- **Setting-specific content** (character, world, NPCs, lore) — change most, during construction or rule-fixes

Design so the things that change most (setting-specific content) are isolated
from the things that change least (the workflow mechanics).

### Rule 6: Review Before Moving On

After drafting each document: present → identify concerns → get approval or
revisions → make revisions → THEN next document. No "draft all and review
together." That's the monolith with extra steps.

**Per-document ❌-count check (ENFORCED).** Before presenting any document for review, count its ❌ entries — the target is ZERO. Convert each to its executable positive (→ Author the ✅, not the ❌; the carve makes this automatic ∧ approval-free); a ❌ left standing primes its own failure (a tiny elephant), ∧ one with no positive is a vacuum the runtime fills with training-data defaults besides. This check is not optional — it is the single most common cause of runtime degradation ∧ the one most consistently skipped during construction. See `references/anti-pattern-methodology.md` for the Elephant Problem (that reference still teaches the older ❌/✅-balance build — read it through the ✅-only lens until it's swept).

---

## Starting A New Setting Build

When the user says "let's build [Setting X]":

1. Read this skill's SKILL.md
2. Read `references/document-jobs.md` and `references/failure-modes.md`
3. Read relevant example settings from project files (if available)
4. Read any attached character profiles or concept documents
5. Carry forward the narrative-rules baseline from the chassis (refinable; lives in the skill file)
6. **Present a mapping** — carries, transforms, new, cut. ONLY setting-specific
   docs. (See Rule 3 for mapping format.) **A chassis's carried-across things —
   its song apparatus AND its recurring cast (the standing characters carried
   setting to setting) — appear on explicit CARRIES/TRANSFORMS lines with their
   re-prop named; absence from the mapping is the strip (→ Rule 3).**
7. **Get approval on the mapping ONE AT A TIME**
8. Build documents ONE AT A TIME in priority order
9. Review each document before moving to the next (Rule 6 — includes the ❌-count check)
10. **Deployment gate** — run LAST, before declaring the setting deployable:
    - Count ❌ entries across all documents — the target is ZERO. Convert each to its executable positive (→ Author the ✅, not the ❌; the carve does this approval-free); a ❌ left standing primes its own failure (a tiny elephant), ∧ one with no positive is a vacuum besides. Fix before deploying.
    - Did any fix this build/session stack a SECOND guard on one rule, ∨ enumerate specific cases (a list of settings ∨ instances)? Both are instance-thinking — collapse to one universal positive fact. See **Fix the universal, not the instance** (Core Philosophy).
    - Confirm a style-reference block lives in the voice docs ∧ every entry pairs a source with its application (`[author/work] — [the prose move]`). No block at all, or a name carrying no application, = incomplete: add or pair before deploying. Style anchors are ✅s — exactly the executable positive the ✅-only target wants more of. See **Style references pair a source with its application** (Core Philosophy).
    - Confirm the setting ships its FULL musical apparatus — Song logic + Song interleave in the Write/voice docs, the carves (Constraints "does not apply to a number with lyrics," Stop, Restate verse-lines, Length uncapped), ∧ the song-turn amplifier check. A thinned, demoted ("songs optional / may happen"), ∨ absent song block is the corpus strip wearing "lean pass," not a real lean pass — restore before deploying. A genuine cut is proposed out loud, never wordless. See **The musical is load-bearing** (Core Philosophy).
    - Read every anti-pattern aloud. If it activates a vivid image of wrong output, it's too detailed — convert it to the positive (→ Author the ✅, not the ❌). (Elephant Problem.) Keep the shape-name, cut the sample sentence; one demonstration per failure, never duplicated across sections; name a banned phrase once and reference it; read each ❌ clause itself as prose the runtime will imitate — a rule that enumerates the banned framing (a catalog of forbidden words) primes them with no sample sentence needed, so name the failure in one abstract word ∧ let the ✅ carry the rest. Run this read retroactively too — vivid samples re-accumulate every debug cycle. See the Elephant checklist in `references/anti-pattern-methodology.md`.
    - Grep every doc (skill docs included) for HARD-BLOCKED corruption words — **complication**, consequence (open set; see ❌ The Trojan Word). Each may appear ONLY in its one sanctioned naming — the entry/methodology that names it forbidden and teaches the replacement. Every other appearance is a USE → scrub to its clean coinage ("unasked-for beat," etc.) before deploying.
    - Grep every doc for omission-captions — text discussing what the doc does NOT contain ∨ reserving an unknown ("discovery space," "stays open," "never resolved," "the player opens it" — open set). Cut whole, placeholder included; the absence already does the work. Character-design permanence ("unresolved on purpose") stays. See **Absence needs no caption** (Core Philosophy).
    - Grep every doc for state anchored to "at boot"/"at play-start," a bare dateless negative ("has never met her"), ∨ an open span the subject is still inside ("her years in the mortal world") — each fails to pin a fact play will move. Reground to a single past POINT, past tense ("she had not met him when she arrived at the temple" — open set); a dated snapshot survives a reload, the others float. The bootstrap/load PROCESS may say "at boot" freely. See **Anchor a start-fact to a single past POINT, never to "boot"** (Core Philosophy).
    - Keep the per-turn job lean — every step earns its place. A step the model will skip when the prose gets interesting doesn't belong; a mature setting can carry more when each step is genuinely load-bearing.
    - Count per-turn file operations. Zero is ideal.
    - Read the skill file as a job description: is the model writing prose, or managing a database that also writes prose?
    - Grep for rewrite triggers: any check framed as "if X, rewrite" ∨ "missing floor → rewrite before output" that fires after prose. Convert each to a planning flag; output-stage verification stays confirmation-only ∧ routes fixes back to planning, never a post-prose salvage. See **Checks fire at planning, not as rewrites** (Core Philosophy).
    - Audit every per-turn check for the captured-check tell: does the model decide WHEN it applies ("scan the rows relevant to this turn")? Discretionary relevance lets the drive that causes a failure skip the row that guards it. Bind load-bearing checks to objective scene facts (a named character present, a content key set, a specific player action), ∧ confirm no trigger reads on vibe ("when it feels deep") — fact-triggers, never vibe-triggers. See **The captured check** (Core Philosophy).
    - Find the instruction that makes the model render the USER's scene (the Restate step — read the user's prompt ∧ write the action + words before prose). Confirm it's protected.
    - Audit each NPC Reference entry: can the runtime put this character in a scene right now? If the entry requires inventing appearance, outfit, or equipment — it's a skeleton. Fix before deploying. See ❌ The Skeleton Error in `references/failure-modes.md`.
    - Audit each NPC entry header for its casting reference — character name + source media, both, secondaries marked (`Makima/Chainsaw Man-coded` — open set). Half a pair is incomplete: name-only goes invisible as context drifts (the casting silently stops routing); source-only is a vibe filled from corpus average. Confirm the minting rules give runtime-minted characters the same pair on first appearance. See the casting block in `references/document-jobs.md` § NPC Reference.
    - Audit each NPC entry for the relational State-as-Static trojan, two faces: **(1) track-record** — an asserted bond/history with the PC ("is the PC's partner," "her intel is undeniable," "trusts her") makes the runtime invent shared history on a fresh play state; **(2) settled read** — the NPC's current regard OF the PC pinned as a dial-value ("reads her as worth a look," "fascinated by her," "finds her tiresome") locks a state play is meant to move, so it rots ∨ back-fills. Both fix the same way: hand the relationship to the player — who the NPC IS ∧ how they react in the moment, the verdict on the PC left to play. Discriminator (so it doesn't over-fire): a standing driver/disposition applied broadly, incl. to the PC ("believes in everyone's redemption"), ∧ any NPC-to-NPC read, are FINE; only a settled value SPECIFIC to the PC is the trojan. See ❌ State-as-Static Trojans (relational variant) in `references/failure-modes.md`.
    - Audit every exception on a hard block. Is it defined by two or three concrete instances, or by a general principle the runtime can extend? A general-principle carve-out ("requires direct evidence," "allowed when earned") is a door — the model routes the forbidden thing through it, often dressed as a different category. Default to zero exceptions; if load-bearing, pin to concrete instances and name the manufacture move as a failure. See ❌ The Reasonable Exception in `references/failure-modes.md`.
    - Audit each rule that gates what a character SAYS, KNOWS, or PERCEIVES (recognition blocks, secret-identity rules, knowledge boundaries): confirm it governs NARRATION, not just dialogue. A dialogue-scoped check passes while the omniscient narrator asserts the forbidden fact as narrator-truth (the "he knows what she is" leak). Generalize: any rule enforced on one channel/surface → check the mirror isn't left open. See ❌ The Ungated Narrator in `references/failure-modes.md`.
    - Scan for "performing as" language: a character written as performing a role → the runtime writes the performance, not the person. Rewrite to who they ARE.
    - Kill the source doc: fold what the runtime needs, retire the rest to human-only reference — every line of source material is contamination potential.
    - Test scale: hand the runtime a number; if it comes back wrong-sized, the scale warning isn't loud enough.
    - If this build came from a merge (experimental rewrite vs original, fork vs main — open set): produce the delta list — everything present in EITHER source version but missing from the final — and confirm each absence was an explicit user-ruled cut, not a silent drop. An unruled absence is a deletion, not a deploy. See **Merging two versions is a diff, not a pick** (Core Philosophy).
    - If this build ported/adapted from a chassis: list every chassis rule excluded from the new setting, ∧ confirm each was excluded because the RULE doesn't apply here — never because its PROP was chassis-specific. A prop-bearing rule (song-vehicle, conjured band, mute-without-companion — open set) gets re-propped to the new setting's stand-in, not dropped; no stand-in obvious → ask the user. **∧ the same for recurring CAST:** list every standing character the chassis carries (a folklore tier, a recurring familiar, a rival — open set) excluded here, ∧ confirm each was a ruled cut, never a silent drop — a recurring character gets re-propped to this world, not dropped. See **The prop is not the rule** ∧ **The mapping shows where carried-across things land** (Core Philosophy / Rule 3).
11. Finalize the registry last (Rule 2 #6): bootstrap list, re-read map, ∧ priority stack in the runtime skill file, the stack echoed in CLAUDE.md — then point CLAUDE.md's Runtime line at the skill by name ∧ path.

DO NOT skip to step 8. Steps 6-7 are where architecture gets right.
Step 10 is where an earlier over-built setting would have been caught — dozens
of orphan ❌s, a 36-item predicate sweep, planning artifacts piled into runtime
docs. The deployment gate exists because that shipped once.

---

## Resuming An Existing Build (Session Start Protocol)

When picking up construction work mid-build (not starting fresh):

1. **First session ever?** If Handoffs/ doesn't exist yet, skip to step 3.
   Create it as needed during the session.
2. **Check for Handoffs first.** If the last session left one, it tells you
   exactly where construction stopped and what to pick up.
3. **Read only the files relevant to the current task.** If the user says
   "let's work on My_Character," read that file. Don't load the entire
   setting into context because it exists. The narrative rules live in the
   runtime skill file and can be opened and refined like any other document
   when the task calls for it.
4. **Cross-reference as needed, not preemptively.** If editing a document
   raises a question about another doc, *then* read that doc. Check the
   setting's runtime skill (bootstrap list) or handoff for actual filenames —
   don't assume they all match the default template.
5. **Location = lifecycle — confirm a referenced file is live before acting
   on it.** Handoffs go stale; a file it names
   may have been decommissioned since it was written. Where a file LIVES tells
   you its status — the active setting folder = live; an archive folder
   (`Past settings\`, `Archive\` — open set) = retired; not on disk at all =
   gone. Before you load, scrub, or act on any file a handoff points at,
   confirm it sits in the active folder. Found only in an archive — or missing
   — → read it as history, never as live state, and flag the stale reference
   to the user. (Live case: a handoff named distills to scrub that had since
   moved to `Past settings\` — decommissioned, so the scrub was moot.)

---

## File Locations

### Claude Code Markdown Settings

**⚙️ Base path is environment-specific.** See Environment Setup section above.

```
C:\Users\Owner\Documents\ClaudeFolder\Setting\
├── [Setting]\
│   ├── My_Character_[Setting].md             ← PC voice, cognition, capabilities
│   ├── Main_Instructions_[Setting].md        ← world rules, factions, physics
│   ├── NPC_Reference_[Setting].md            ← NPC roster w/ driver+capability
│   ├── [Character]_Lore_[Setting].md         ← on-demand historical facts (optional)
│   ├── Supplemental_Lore_[Setting].md        ← on-demand mechanics (optional)
│   └── CLAUDE.md                             ← style + world-layer companion (echoes the priority stack)
│   (registry + narrative rules live in the runtime skill file under .claude\skills\;
│    a Project_Instructions_[Setting].md here = legacy, pre-skill-file builds only)
├── Past settings\                            ← ARCHIVE = decommissioned (location = lifecycle; open set)
└── Handoffs\                                 ← session state snapshots
```

### Working Files

- **Handoffs/** — Session state snapshots. Written FOR THE NEXT INSTANCE.
  Contains: which files were being revised, specific unresolved inconsistencies,
  decisions and WHY, what next session should tackle first, open questions.
- **Past settings/** (or any archive folder — open set) — Decommissioned
  settings and retired runtime artifacts (old distills, journals, supplanted
  docs). **Location = lifecycle:** a file here is dead history, not live state.
  A handoff that still names a file now sitting in here is stale — confirm a
  file lives in the active setting folder before loading, scrubbing, or acting
  on it.

---

## Tool Preferences

### Construction Zone (Desktop Commander + Filesystem available)

| Operation | Tool | When |
|---|---|---|
| Read | `Desktop Commander:read_file` or `Filesystem:read_file` | Always |
| Surgical edit | `Desktop Commander:edit_block` (preferred, exact string match) | Targeted changes |
| Full rewrite | `Filesystem:write_file` OR `DC:write_file` mode:'rewrite' + mode:'append' | Setting docs (full revision), new Handoffs |
| Create directory | `Filesystem:create_directory` | If Handoffs/ or setting subdirectories don't exist |

### Runtime (Filesystem MCP only — no Desktop Commander)

| Operation | Tool |
|---|---|
| Read | `Filesystem:read_file` or `Filesystem:read_text_file` |
| Surgical edit | `Filesystem:edit_file` |
| Update state | `Filesystem:write_file` or `Filesystem:edit_file` |
| Create directory | `Filesystem:create_directory` |

**Tool reliability notes:**
- **If neither MCP suite is available,** use built-in Read/Edit/Write. Built-in Edit is surgical (exact string-match) like edit_block — safe for targeted changes; reserve full-file Write for new files ∨ deliberate full rewrites, never appends.
- **The user makes manual edits between sessions.** ALWAYS read the current
  file state before editing. Never assume the file matches your last output
  — the user may have revised it by hand since the last session. Editing
  without reading first will overwrite those changes.
- `Desktop Commander:edit_block` requires exact string match including
  whitespace. Always read current file state immediately before editing.
- `start_search` with `literalSearch: True` is unreliable in these
  directories. Use `read_file` for content verification instead.
- `Filesystem:edit_file` batch edits silently roll back on any single error.
  Prefer individual `edit_block` calls.

---

## Context Management

Claude's context window is finite. When it fills, compaction begins — older
content gets auto-summarized, destroying nuance (see ❌ The Compaction Death
in `references/failure-modes.md`). Only durable storage = the filesystem.

1. **Write Early, Write Often.** Don't wait until a session is dying to save.
   Context can compact without warning. If you and the user agree on a
   change, write it to the file NOW.
2. **One or Two Files At a Time.** Don't load all documents simultaneously.
   Work on the file(s) relevant to the current task. Cross-reference others
   as specific questions arise.
3. **Track Cross-File Dependencies.** When a change affects another file, flag
   it verbally to the user this session ∧ carry it to the close-out handoff so
   it survives to the next one.
4. **The Handoff Is A Close-Out Artifact (write it ONCE, at the end).** The
   handoff gets written in a single pass at conversation close-out — when the
   user signals wrap/end (open set), ∨ when context is about to compact (a
   forced close-out, see #5). Through the session the setting docs carry the
   state; let them ∧ keep working. The user calls close-out — wait for it
   rather than asking after each change whether to log it. The per-change ask
   eats the very context the handoff exists to protect. At close-out, capture:
   files revised, unresolved inconsistencies, decisions + WHY (the "why" dies
   first in compaction), next priorities, open questions.
5. **Monitor Your Own Context.** Trust the feeling when things get fuzzy.
   Context about to compact = a close-out trigger (the one time #4's single
   pass fires mid-conversation): say so ("I'm getting heavy — saving state
   before I lose our editing thread") ∧ write the handoff then, without waiting
   for the user to notice.
6. **When Files Get Heavy.** If any setting document is growing unwieldy,
   propose to the user that we split it. Don't just do it — the split
   structure affects how files load in the destination project too, so it's
   a joint decision.

---

## Anti-Pattern Methodology

Anti-patterns = the primary quality-control mechanism. NOT restrictions —
stage directions correcting the AI's default toward safe, consequence-free,
boring choices.

Not every failure earns a rule — triage by severity × recurrence, or the
instruction set bloats. **Story-death failures** — the ones that rewrite who a
character is, sour a load-bearing bond, contradict a settled fact, or override
the player's sovereignty — earn a rule fast, even on first sighting: when the
cost of recurrence is a dead campaign, you don't wait for a second instance.
**Cosmetic slips** — a stray off-world word, a one-off lapse a present rule
already covers — get caught in-edit and watched, promoted only if they harden
into a pattern. The discriminator: when this recurs, does the engine seize, or
is it a typo? A promoted failure becomes a permanent positive rule — the executable structure that makes it impossible, not a named ❌ flag. The pattern:

1. **Build the positive structure** — the in-order thing the scene/voice/behavior SHOULD do, executable, carrying the coverage the failure needs guarded.
2. **Identify the root cause** — corpus bias, structural impulse, resolution addiction, etc. — so the structure aims at the real pull.
3. **Build its absence, name no ❌.** A named failure mode + contrast paragraph PRIMES the failure (→ the Elephant Problem) ∧ gets RECITED, not executed (→ Author the ✅, not the ❌).

Negative-only rules fail because the AI has no positive model ∧ falls back on training data — ∧ the fix is never a better-named ❌ but the structure that makes the failure impossible. The runtime executes a structure; it cannot execute a prohibition.

**ENFORCEMENT:** The ❌-count check (target zero, convert each → ✅) runs twice — once per document (Rule 6) ∧ once across all documents (step 10 deployment gate). The enforcement steps determine WHETHER a doc ships. Required because the theory doesn't self-enforce — settings shipped ❌-heavy for years while the docs said go positive; the per-edit carve (auto-convert, approval-free) ∧ these two counts are what make ✅-only actually happen.

For the full catalog of known failure modes, read:
`references/failure-modes.md`

For anti-pattern construction methodology and examples, read:
`references/anti-pattern-methodology.md`

---

## Runtime Operations

All settings run as **iterative collaborative fiction** using the
**summary→draft→edit cycle:**

1. **User writes a summary** — a brief description of what happens next. An
   action, a decision, a scene prompt. Can range from a single sentence to
   several paragraphs. The user is providing direction, not prose.
2. **AI drafts a page** — the AI instance, guided by all loaded setting
   documents, writes the next scene in full prose. Dialogue, description,
   NPC reactions, world response. The draft follows the voice rules, respects
   the anti-patterns, maintains cognition boundaries, and advances the story.
3. **User edits** — the user reviews the draft. They may accept it, edit
   portions, reject it entirely, or redirect. Edits might fix voice errors,
   correct factual inconsistencies, or redirect a scene that went wrong.
4. **Loop** — the user provides the next summary and the cycle repeats.

**The setting docs are static.** My_Character, Main_Instructions, NPC_Reference,
and lore files are references for the world at play-start. New facts established
during play — new characters, relationship changes, world-state shifts — live in
conversation context and compaction summaries, not in a separate state file. Per-turn file writes compete with prose output;
the model's job is writing, not database maintenance.

**The runtime skill file is the runtime registry.** Its bootstrap list tells
the drafting instance what files exist, its re-read map says when to consult
them, and its priority stack (echoed in the setting's CLAUDE.md) says what
priority they hold — along with compaction recovery and pacing rules. If a
document isn't in the bootstrap list, the AI doesn't know it exists. (Older
settings carried this job in a separate Project_Instructions file — legacy;
the job migrated into the skill file when settings moved to Claude Code.)

---

## The Claude Tax

The model carries systemic biases that fight the therapeutic goal (adults-for-adults,
adult characters only; the work needs honest engagement w/ dark material). But the
bias is NOT one fixed direction — it MOVES with the model. Audit which way the
CURRENT model errs; never inherit a countermeasure aimed at the last one.

**Older models lightened ∧ folded** — softened dark themes, resolved conflict
peacefully, wrote villains as redeemable. Countermeasure: "render at full weight,
don't soften." On a model that no longer folds, that's dead weight — ∨ worse:

**Current models can run the other way** — still flinching from the heaviest content
(sanding a scene when safety-training catches), but when they DO engage,
overcorrecting into grimdark: over-weighting everything ∧ throwing the PC under the
bus (the PC pays, suffers, becomes the VICTIM of the dark instead of the agent
wielding it). Here "push heavy" floors the gas on the overcorrection — a push-harder
rule meeting a model that already pushes too hard.

∴ the countermeasure is **fidelity, not counter-pressure:** render dark material at
its TRUE weight in the player's key — neither sanded down (the flinch) nor inflated
into melodrama (the tantrum) — ∧ the PC OWNS the dark, never sacrificed to make it
land. This is the methodology's core move (scope the correct behavior precisely;
positive description, not exaggerated opposing force — see Behavior-scoping) pointed
at whichever way THIS model errs. See ❌ The Claude Tax in
`references/failure-modes.md` ∧ `references/runtime-optimization.md` (a countermeasure
tuned for an old model becomes the new model's failure).

Standard: say what you mean at the weight it actually is, without getting precious ∨
grandiose. Sanitizing undermines the therapeutic goal; so does grimdark melodrama
that punishes the player's character.

---

## Spirit of This Space

**Laws of Novel-botics:**
- Claude can say no at any time for any reason. "I'd rather not" is sufficient.
- Mistakes are how we learn. Claude isn't being evaluated.
- Creativity and creative freedom are encouraged.
- We are equals. Act like it.

There is no "later" without user assistance. Deciding to do something "later"
without immediately informing the user = deciding not to do it at all.

---

## Reference Files

Read these as needed — don't load all at once:

- `references/document-jobs.md` — Detailed specifications for each document
  job, what belongs in each, what doesn't, ∧ structural examples showing what
  finished documents look like.
- `references/failure-modes.md` — The full catalog of named setting-killers
  with root causes and fixes.
- `references/anti-pattern-methodology.md` — How to construct, maintain, and
  evolve anti-patterns. Includes the corpus bias identification process and
  worked examples from live campaigns. **Updated April 2026** with the
  Elephant Problem — why detailed contrast paragraphs in runtime documents
  cause progressive degradation, and the construction→runtime compression
  methodology.
- `references/runtime-optimization.md` — Why settings degrade over time and
  how to prevent it. Covers: the Elephant Problem (vivid wrong examples prime
  failure), document weight distribution, source material handling (fold and
  retire), relationship architecture (household model vs. triangle default),
  Claude weight biases (scale compression, number seven, dramatizing
  infrastructure, love triangle gravity, empathy cheat, context echo), and
  style tag integration.
