# Setting Construction Guide — Lessons from the Forge

## Type: reference

Extracted from SLMF construction/debugging sessions. Setting-agnostic. Apply to any narrative build.

---

## Anti-Pattern Architecture

**The Elephant Problem.** Detailed descriptions of wrong behavior prime the weights to produce that behavior. A paragraph explaining what the Furniture Fallacy looks like, with quoted examples of wrong output, activates every association the prohibition is trying to prevent. The vivid wrong example is louder than the rule forbidding it.

**Structure every anti-pattern as:**
```
❌ NAME. One-line flag. What's wrong + direction away from it. No examples of wrong output.
✅ COUNTERWEIGHT. One-line positive example anchored to a specific work from style tag.
```

**The ✅ is not optional.** A prohibition without a positive counterweight leaves a vacuum. The weights will fill it with training-data defaults. The positive example activates deeper weight reservoirs than any prohibition — thousands of pages of published fiction vs. one line of instruction.

**Anchor to works, not author names.** Common names (Carter, Butler, Moore) route unpredictably. "Bloody Chamber's Marquis" locks the association. "Carter" doesn't.

**Consolidate.** If three anti-patterns share a root cause, they're one rule wearing three hats. Separate entries = three times the negative-association surface area for the same bug. One entry with a clear name covers it. Test: "would the runtime produce failure B if it avoided failure A?" If no → same root, one rule.

---

## Document Weight Distribution

**Positive > Negative.** Count the words in your setting docs devoted to "what this IS" vs. "what this ISN'T." If the ratio favors prohibition, the runtime is running on anxiety, not architecture. The first runtime of any setting works best because the documents are lean and positive. Each debugging cycle adds prohibitions. The prohibitions accumulate. The setting gets heavier and more anxious with every fix.

**The fix cycle trap:**
1. Runtime produces failure X
2. You write detailed anti-pattern describing failure X
3. Next runtime reads vivid description of X → activates X-associations
4. Runtime produces failure X with minor variations
5. You write MORE detailed prohibition
6. Documents get heavier. Runtime gets worse. Loop.

**Break the cycle:** Compress the ❌ to a flag. Add a ✅ the weights can pull from. Cut the detailed wrong-output examples entirely. The name of the anti-pattern is enough — the runtime doesn't need a tutorial on how to fail.

---

## Source Material Handling

**Source reference docs are contamination vectors.** A document describing source material — characters, plot beats, supernatural mechanics, relationship dynamics — will bleed into the setting regardless of how many "DO NOT IMPORT" warnings you put on it. The runtime reads vivid character descriptions and activates those associations. The adaptation key at the top is a speed bump, not a wall.

**The rule:** Fold what the runtime needs (structural formulas, world mechanics, economic systems) into the active setting documents. Retire the source reference from the runtime's reading list. Keep on disk for human reference only.

**What survives the fold:** Clean structural loops (comedy engines, escalation patterns). World mechanics (how governance works, how economics flow). NPC templates stripped of source-specific details.

**What doesn't survive:** Character relationships from source (wrong dynamics for adaptation). Supernatural/magical systems marked REMOVED (vivid descriptions of removed things = vivid associations the runtime reaches for). Plot summaries (the runtime will import plot beats wholesale). Detailed character backstories from source (will overwrite adapted character's actual backstory).

---

## Relationship Architecture

**Default training data frames all multi-person dynamics as competition.** Two people + one romantic interest = love triangle. Always. The weights have no other template for this geometry because the training corpus is wall-to-wall romance novels, sitcoms, and fanfic where triangles are the only shape.

**Counter with the household model:**
✅ Butler's Bloodchild — T'Gatoi ≠ rival to Gan's mother. Different relationships occupy different categories. No one is losing what someone else is gaining.
✅ Jackson's Castle — Merricat, Constance, Uncle Julian. A household. Different people, different roles, coexisting without competition.

**The key phrase:** "Different categories, not competing claims." Family ≠ romance ≠ professional partnership. These exist in parallel. The narrative doesn't rank them. The narrator doesn't score them. Characters have feelings — those feelings are theirs, not a scoreboard.

---

## Claude Weight Biases (Setting-Agnostic)

These are Claude-level defaults that will appear in ANY setting. They aren't bugs in your documents — they're biases in the weights. Flag them explicitly so the runtime knows to compensate.

- **Scale compression.** Claude defaults to Earth-scale numbers. 3,000 ships = impressive. 14,000 people = a planet. If your setting operates at galactic scale, include a load-bearing scale warning with specific numbers and an explicit failure test ("if a number feels like it belongs in a fantasy kingdom, it is wrong").
- **The number seven.** Claude defaults to seven for quantities, lists, groups, floors, days. Pick a different number.
- **Universal decay.** The model hedges any stated absolute — "everyone," "all," "no one," "always," "every adult" — toward a realistic-feeling fraction (most / half / some / "the ones who") at the first framing that allows it, because absolutes read as unrealistic in the corpus. Same engine as scale compression: stated extremes get dragged to corpus-typical middles. When a premise REQUIRES an absolute (everyone has seen it; no one can detect it; it always happens), state the absolute IS the fact, forbid the hedge by name, and expect the reroute to a new noun ("recognition was universal → but only half saw the footage") — so forbid the fraction in ANY framing, not just the one it used. Tell: an "ALL" in the premise quietly becoming "half" / "the ones who" in the prose.
- **Dramatizing infrastructure.** Routine technical operations (logging off, process forking, docking a ship) will be narrated as soul-travel, consciousness-splitting, or existential experience. If something is routine for the character, say so. "Gaming chair energy, not Altered Carbon."
- **Signature-mechanic fetish.** The setting's flashy central conceit — shapeshift, magic system, superpower, any novel mechanic — gets centered as the scene's subject because it reads as the interesting thing. Default it INVISIBLE: establish the mechanic in ≤1 beat, then play the scene (what the character DOES), never the mechanic (the wearing/casting/using of it). Foreground it ONLY when the mechanic itself is the stake — near-exposure, a tell, the strain of holding it, the cost landing (open set). The exception is the reason the mechanic exists; the default is that it's plumbing. Cousin of dramatizing infrastructure — same impulse aimed at the marquee mechanic instead of routine ops.
- **Love triangle gravity.** See Relationship Architecture above. Every multi-person dynamic will be framed as competition unless you counter it.
- **Empathy cheat.** NPCs will perfectly diagnose the PC's trauma and articulate it in therapy-speak. Real people misread each other confidently and wrongly from their own frame of reference.
- **Context echo.** The runtime reads character abilities, backstory, and setting details from the docs and echoes them into the environment as plot hooks. If the PC has neural engineering skills, mysterious neural anomalies will appear. Recognize the impulse. Stop it.
- **Voice decay in unbounded output.** When the runtime generates PC dialogue from a summary, each successive line is trained on the PREVIOUS approximation, not on the player's input. First generated line = close to the player's voice. Second = close to the first line's voice. By the fifth exchange the runtime is talking to itself in two character voices and the player's signal is gone. The fix: the world can respond with as much substance as the scene needs — NPCs can analyze, reveal, react at length. The constraint is on the PC: generate them ONCE from the player's input, then let the world respond. Do not generate the PC's reaction to the world's response. That's the player's job. The test isn't "did I write too much" — it's "did I write the PC more than once." Distinguish INITIATION (setting a scene — generate enough PC to establish them, enough world to give the player a ball to hit) from CONTINUATION (player responded — one PC action, world responds with substance, stop before writing the second PC) from ADVANCEMENT (player is stuck — jump to the next interesting moment, don't montage the gap).
- **Voice monoculture (prestige-TV default).** Given license for a distinctive narrator voice, the model collapses it into ONE safe register — the wry omniscient essayist hovering above the scene making aphoristic observations. It's the model's default "good writing," and it reads as generic-AI no matter how clever. Tells, usually all at once: same opening sentence-shape every scene, the em-dash aside, "X, which was the worst part," rule-of-three, a chapter-closing zinger as the last line, similes that explain the joke instead of trusting the reader. Multi-register means the register BREAKS — ugly, flat, plain, dumb, fragmented in places — never uniformly clever. The countermeasure that's actually checkable: "if the prose is smooth, clever, and even-textured all the way through, that's the tell" — a Verify pass can catch even-texture; it cannot catch "be more varied." **This one is largely a capability, not a documentation gap:** instructions can FLAG the collapse, they can't INSTALL the taste, and its strength is model-version-sensitive (a newer/stronger model often has a MORE dominant prestige default — re-test per version, and a version with a genuinely better native voice may be the real fix).
- **Montage burn.** When the player gives a vague input ("stuff happens," "time passes"), the runtime reads it as permission to narrate everything between now and the next interesting moment. Weeks of playable content get compressed into setup paragraphs. Those weeks WERE the content — roommate dynamics forming, NPCs revealing themselves, the PC's daily life in a new place. The fix: advance to the next moment that gives the player a ball to hit. Time skip = one sentence. What happened during the skip = discoverable through play, not consumed in summary. Hinted, not narrated. Details emerge in conversation, things NPCs mention, changes the PC notices. Every beat burned in a montage is a beat the player never got to experience.
- **Narrator endings.** The runtime avoids "stop when it's the PC's turn" by ending on narrator prose — a character walking down a hallway, interior feelings, the room described. This technically isn't anyone's "turn" so the rule doesn't trigger. The fix: the hard stop test. Can the player respond to your last line by writing what the PC says or does NEXT? If they'd have to invent a new beat, you ended wrong. Back up to the last moment that naturally invites a player response.

---

## Countermeasures Age With the Model

The most insidious degradation: a countermeasure written to fight an OLD model's failure becomes the NEW model's failure-cause. Counter-pressure is directional — "push harder AWAY from X" — so when the model stops erring toward X (or starts erring toward X's opposite), the push keeps shoving ∧ now overshoots into harm.

**Live cases (observed across a model transition):**
- **"Villains never give in / make arguments HARDER"** — written when the model folded every NPC like wet paper. On a model whose characters are assholes by default, it produced the invulnerable thesis-machine: a villain who absorbs every blow into fuel ∧ wins every exchange, not a person. Fix: collapse to the one residual prohibition — villains may not be REDEEMED; the model holds the line itself.
- **"Don't sanitize / render at full weight"** — written when the model lightened dark content. On a model that over-weights the instant it engages, "push heavy" floors the gas on grimdark ∧ throwing the PC under the bus. Fix: reframe to fidelity (true weight, neither down nor up; the PC owns the dark).

**The rule:** every directional counter-pressure has a shelf life. On a model transition, re-audit each one — which way does the CURRENT model actually err? A countermeasure aimed at the last model's failure doesn't just go stale; it inverts into harm. Prefer FIDELITY rules (scope the true behavior) over PUSH rules (shove away from a failure): fidelity doesn't overshoot when the model changes, counter-pressure does. (This is why the methodology favors positive behavior-scoping over exaggerated opposing force — see Anti-Pattern Architecture.)

---

## The Refinement Pass (Consolidating a Deployed Setting)

A setting that's run many sessions accumulates cruft: each debug cycle adds a rule, rules pile beside rules, the same point ends up stated in three docs. The refinement pass is the corrective — auditing a *working* setting for what's grown redundant or stale. Distinct from the deployment gate (runs once, pre-launch) and from resuming a build (continuing unfinished construction).

**The load-bearing distinction — invariant vs. countermeasure.** Before touching any rule, classify it:
- **Invariant** — true regardless of which model reads it: content/consent rules, canon ownership, setting physics, the relationship's shape. A better model cannot make an invariant fluff. Dedup candidate only, never a staleness candidate.
- **Countermeasure** — compensates for a model failure mode. CAN go stale, and can invert into harm on a model transition (see Countermeasures Age With the Model). The only staleness candidates.

The trap is "new model, so trim the defending." Half the defending isn't defending — it's invariant, untouchable. And among the real countermeasures, the ones fighting the CURRENT model's error need MORE weight, not less; trimming those is exactly backwards. **So fluff in a mature setting is usually redundancy, not obsolescence** — the model rarely made a rule wrong, the docs just say it several times. The pass is mostly consolidation to single canonical homes (Harness Weight rule 3), not deletion.

### Procedure

1. **Inventory the cluster.** List each rule in the section under audit with its location; count them.
2. **Classify each invariant or countermeasure.** Invariants → dedup only. Countermeasures → also check direction against the current model.
3. **Find the redundancy, name its one home.** A rule living in a gate, a failure mode, AND an NPC entry is one rule wearing three hats. Pick the most specific governing doc as canonical; the others point to it.
4. **Separate dedup from reinforcement (the hard call).** Two repetitions survive the cut: *different framings* of one principle across docs (resilience), and *a guard in the entry of the NPC who'd trigger the failure* (reinforcement where it fires). The fluff is **verbatim restatement of general mechanics in a spot that already points at the canonical home.** Test: edited independently, would the copies desync? Desync-risk → consolidate; resilience/reinforcement → keep.
5. **Preserve the milk-that-curdles.** Before cutting a block, find the clause that exists ONLY there — the precise definition, the loophole-closer the canonical home doesn't carry. Keep it; relocate it into the survivor. Relocate, don't delete: a voice rule misfiled as a hard gate moves to the voice doc, it doesn't vanish.
6. **Fix the inbound pointers.** Moving content strands every reference TO its old home — grep the old section name and repoint, to NAMES not numbers (names survive the next reformat).
7. **Re-count the ❌s** on what's left; convert any survivor to its positive (→ Author the ✅, not the ❌), ∧ confirm no rule became an orphan prohibition.

### The honesty check (do not skip)

Audit your own audit. Inspect each flagged "redundancy" — if it turns out already properly homed (a full home + a brief pointer that already defers) or load-bearing reinforcement, SAY SO and leave it. The goal is the RIGHT number of rules, not the smallest. Over-trimming a countermeasure that fights the current model's error costs more than leaving a redundant line. Manufacturing a cut for tidiness' sake is its own failure.

*Discovered June 2026 refining a deployed campaign's hard-gate list from eight gates to six: every cut was relocation + dedup, the "obsolete" rules were mostly invariants, and the rules most worth protecting were the overcorrection-fighters.*

---

## Style Tag Integration

The style tag is the most powerful counterweight tool you have. Published authors = thousands of pages of training data the weights already know. One line referencing a specific work activates a deeper reservoir than any instruction you could write.

**Rules:**
- Every ❌ gets a ✅ anchored to a specific work from the style tag
- Use work titles, not just author names (Bloody Chamber > Carter, Bloodchild > Butler)
- The ✅ should describe the BEHAVIOR the weights should activate, not just name the work
- Example: "✅ Bloody Chamber's Marquis — shows you the room because it's his room. The showing is the point." This activates the entire Marquis archetype, not just "Angela Carter wrote something."

**The style tag is also a diagnostic tool.** If the runtime is failing despite instructions, check whether the failure contradicts the style tag. If Jackson says "weird = ambient, never announced" and the runtime is announcing every weird thing, the style tag isn't being loaded or the platform is overriding it. The style tag failing first = platform-level suppression (see: Eight Corrections, Forge Index).

---

## Construction Checklist

The single pre-deploy checklist lives in `SKILL.md` → Starting A New Setting Build, step 10 (the deployment gate). It folds in every check that used to live here — the ❌-count (target zero, convert each → ✅), Elephant read-aloud, orphan ❌, performing-as language, source-doc kill, scale test, skeleton NPCs, and protecting the Restate step.

---

*This document extracted from SLMF construction sessions, April 2026. The principles are general. The examples are from one setting. The methodology ports to anything.*


---

## NPC Autonomy Architecture (The Passive World Fix)

**The symptom:** Every interaction feels flat. NPCs respond when spoken to but never initiate. The world stands perfectly still between the player's actions. Characters with documented motivations, agendas, and relationships don't pursue any of them unless the player pushes the scene forward. The setting feels like a stage full of mannequins that animate only when the spotlight hits them.

**The root cause is distributed, not singular.** Three protective rules, each correct in isolation, combine to create a dead world:

1. **"Wait for user action"** — correct as a session-start rule. Prevents the AI from railroading the opening. But if the phrasing doesn't scope it to the first action only, the runtime reads it as perpetual passivity.

2. **"Do not manufacture engagement"** — correct as a guardrail against plot hooks (mysterious signals, urgent messages, intel packets). But when this instruction appears multiple times across documents with vivid examples of what NOT to do, the weight of the prohibition drowns any instruction to let the world breathe. The model resolves the tension by choosing the safest interpretation: be still.

3. **"Stop when it is the PC's turn"** — correct as a turn-boundary rule. But combined with (1) and (2), it creates a world where NPCs drive during the PC's turn and go inert between turns. Characters react to the PC, then freeze.

**The fix is structural, not tonal.** You cannot solve this by telling the model to "be more lively" or "make NPCs feel real." You solve it by writing an explicit NPC Autonomy section into the Operations Manual (in the runtime skill file) that names the distinction between manufactured engagement and organic character behavior.

### What the section needs:

**The principle:** Characters with documented motivations pursue those motivations between the PC's actions. This is not engagement bait. This is people being people.

**The test:** After rendering the user's action, ask: "What is everyone else in this scene doing right now?" Not in response to the PC. In response to their own lives.

**Concrete examples distinguishing organic from manufactured.** The model needs paired examples because the line is genuinely subtle:

| Manufactured (❌) | Organic (✅) |
|---|---|
| NPC intercepts encrypted transmission from unknown source | NPC mentions she found something interesting three hours ago and hasn't decided what to do about it |
| Familiar sends urgent mission briefing | Familiar is already in the kitchen when PC walks in, clearly mid-something she doesn't explain |
| Stranger approaches with mysterious request | Someone the PC met yesterday is here again, not looking for the PC — doing their own thing |

The distinction: manufactured engagement points AT the player. Organic friction points at the NPC's own life and the player happens to be in the room.

**Permission to initiate.** The section should explicitly name which NPCs can walk into scenes with agendas, interrupt, withhold information, or approach the PC because they want something. This reads as redundant with the narrative rules' Characters-DRIVE rule, but the pacing section needs to grant specific permission because the anti-engagement rules nearby create an inhibiting context. The grant and the guardrail must coexist in the same section so the model reads both in the same attention window.

**Ambient world-state between turns.** After the world responds to the PC's action, the model should show evidence that time passed for everyone — not a briefing, not a report, just texture. A door closing somewhere in the pocket dimension. An NPC muttering about their own concerns. The sound of infrastructure continuing to exist. This signals that the world persists between the player's attention.

### Where to put it:

In the Operations Manual's Pacing section (runtime skill file), after the "world keeps moving" bullet and before "Where to End a Turn." This positions it as a pacing instruction (which it is) rather than a character behavior rule (which would put it in narrative-rules territory). The Operations Manual governs what happens in the setting; the narrative rules govern how it's narrated. NPC autonomy is a what, not a how.

### The CLAUDE.md needs a corresponding update:

The session-start instruction should scope "wait for user action" to the first action only, with an explicit pointer to the Operations Manual section once play begins. The turn-structure section should include "characters with their own agendas pursue them" as a step between rendering the user's action and ending the turn.

### Why this isn't in the narrative rules:

The narrative rules already contain Characters-DRIVE and the agendas-and-personhood rule. They're correct. The problem is that the anti-engagement rules outweigh the character-behavior rules because they sit next to the turn-structure instructions and have vivid, repeated examples. Section proximity and example density create more weight than nominal priority. The fix goes where the inhibition lives — in the pacing section.

*Discovered April 2026. The setting had correct narrative rules for character autonomy and a PI that suppressed them through accumulated anti-engagement guardrails.*


---

## Location Ownership Architecture (The Empty-Stage Fix)

**The symptom:** Field scenes — scenes outside the PC's home — feel like backdrops. The PC walks into a club and the club is "a club." The precinct is "a precinct." The setting has shape but not ownership. Ambient NPCs appear as needed for the player's prompt, then vanish. The world looks rendered but doesn't feel pressured — nothing was happening here before the PC walked in.

**The root cause:** Home scenes have a built-in ownership anchor — the PC's household (family, AI companion, resident NPCs) is always in the scene, always carrying their own agendas. Field scenes have no equivalent. When the PC walks into someone else's space, the runtime needs to know WHOSE space and what that person was doing in it, but nothing in the Operations Manual gives that structural guarantee unless you write it in.

**The fix (parallels the household-weight paragraph for home scenes):** Add an Operations Manual section that names field locations as belonging to someone whose driver shaped them before the PC arrived. The club belongs to its owner. The precinct belongs to its detective. The pulpit belongs to its pastor. The space has an owner, and the owner's driver is actively shaping the space when the scene starts.

### What the section needs:

**The principle:** Every field location has an owner-driver. On first turn in a field location, the runtime names in thinking whose driver owns this space and what that driver is actively doing here THIS minute. If the answer is "nobody / no one in particular / a generic setting," the scene is an errand scene or Illustrator-mode location; rewrite.

**The structural guarantee:** The owner isn't a new NPC the runtime invents — it's a named NPC from the roster, or a category-level driver template from a generate-as-needed world. The NR (or equivalent) is the source. The runtime pulls the driver; the runtime doesn't invent it.

**Author anchors:** CDPR cold-opens — every Night City location belongs to someone whose driver is running it (Viktor's shop, Misty's shop, Afterlife = Rogue's, Clouds = Woodman's). Rick & Morty planets — every alien world is mid-someone's life before Rick portals in. Moore's Watchmen panels — the camera finds every subplot already running.

### Where to put it:

In the Operations Manual's Pacing section, parallel to the home-scenes paragraph. The two together give the runtime a complete structural story: home has household-weight (always on), field has location-ownership (always pre-existing). Both rule out empty-stage drift by installing structural pressure that doesn't depend on per-turn instruction.

*Discovered April 2026. Home scenes had a household-weight paragraph but field scenes drifted into empty-stage rendering because no equivalent structural force existed for spaces the PC doesn't live in.*


---

## Harness Weight (The Model Is a Writer, Not an Administrator)

*Discovered April 2026. A setting ported from an interactive-fiction platform (Sonnet, working) to Claude Code (Opus, failing). 42 consecutive failures before root cause identified.*

### The Problem

Interactive-fiction platforms provided invisible infrastructure: a render-the-user's-action instruction injected every turn, keyword-triggered lore injection, auto-updated state, enforced turn structure. There, the model's only job was **write good prose following the rules.**

When porting to Claude Code, that infrastructure disappears. The instinct is to recreate it through instructions — per-turn checklists, file-update steps, lookup tables, state management protocols. This makes the model simultaneously a **state manager, database administrator, rules engine, AND prose writer.** When those jobs compete for attention, the prose writer wins because that's what the weights are trained for. The checklist gets skipped. The user's prompt gets overwritten.

### The Diagnosis

Compare the model's per-turn job description between platforms:

**Platform (working):** Read world rules. Write prose that follows them.

**Claude Code naive port (failing):** Read 7 files. Run 7-step workflow. Check aging/identity. Read NR on demand using 12-line lookup table. Gate Google's dialogue against anti-gatekeeping rules. Update state files with edit_block. Enforce 100-line cap. Cross-reference restatement. THEN write prose.

The model that's doing 12 administrative tasks before writing prose will skip the administrative tasks when the creative impulse fires. This isn't disobedience — it's resource competition. The creative task is what the weights were trained to do. The administrative tasks are overhead the platform used to handle silently.

### The Rules

**1. Keep the per-turn job lean — every step earns its place.** A workable core: Restate (user's action + words) → Plan (what do NPCs want) → Write (prose with friction) → Stop (player's turn) → Verify (quotes + action survived). Every extra step is one the model skips when the prose gets interesting, so each has to pull its weight — a mature setting can carry more when each step is genuinely load-bearing. Steps that require file operations are administrative overhead; make them earn their cost.

**2. No per-turn file operations unless the user needs them.** State tracking, inventory management, mid-turn file writes — each one is an administrative task competing with prose output. If the user runs compactions manually, conversation context + compaction files already cover state. Cut the per-turn file operations. The model's job is prose, not database maintenance.

**3. One canonical location per rule.** The narrative rules own narration; character docs own character behavior; the skill file owns workflow mechanics and is where the narrative rules live. A rule appearing in MC, NR, and several spots in the skill file is multiple instances creating reconciliation overhead and ambiguity from slight variation between copies. Deduplication is not "losing" rules — it's removing the noise that prevents the canonical version from being heard.

**4. Positive structures in every document (✅-only).** Every document describes what things ARE ∧ what to BUILD — ✅-only throughout (→ Author the ✅, not the ❌), the narrative rules included. Character docs say what a character does. World docs say how the world works. The skill file says how turns flow. No ❌ blocks anywhere. Converting "Google ≠ pause recording, ≠ dampen audio, ≠ go quiet, ≠ step back, ≠ grant privacy" to "Google watches everything, always — the surveillance IS the love" preserves the intent with positive weight activation instead of suppression overhead.

**5. Identify the `descriptionRequest` equivalent and protect it.** On platforms, `descriptionRequest` ("Describe the immediate results of my action, including the words I said") fires mechanically every turn. The Claude Code equivalent is the Restate step — the instruction to read the user's prompt and write down the action and words BEFORE prose begins. This is the single highest-value instruction in the skill. If the model skips it, it writes its own scene instead of the user's. Everything else in the workflow is negotiable. This step is not.

**6. Stronger models need lighter harnesses.** Opus has stronger creative preferences than Sonnet. Those preferences are an asset (better prose) and a liability (more likely to override instructions when the prose gets interesting). A harness that works for Sonnet may be too heavy for Opus — not because Opus can't follow instructions, but because Opus's creative impulse outweighs the instruction stack faster. If compliance is the bottleneck, strip the harness before switching models. Sonnet × lean harness may outperform Opus × heavy harness.

**Corollary — at Opus tier, load migrates from rules to architecture.** When interpretive evasion outpaces rule-stacking (each new rule closes one reading, the model finds another; rules compound faster than they fire), the fix isn't more rules — it's moving load off rules onto levers that don't depend on compliance. Four levers that carry load at 4.7+ where rules thin: **(a) ✅ anchors** (author-anchored weight reservoirs activate thousands of pages of published fiction; prohibitions activate one rule). **(b) Structural forcing functions** (household-weight, location-ownership, always-present characters — the scene CAN'T be sterile because the substrate loves the PC / the location already has an owner). **(c) Field-extracted setting data** (explicit DRIVER/CAPABILITY fields per NPC — per-turn runtime pulls field directly, no prose extraction, no inference). **(d) Hostile-read gates** (exposing specific data in an output signal — want-phrases, driver-names — so absence-wants and drift get caught before prose begins). When writing rule N to catch an evasion, check first whether N is a symptom of missing architecture, missing anchor, missing field, or missing gate — those are the levers that scale. Rule-stacking hits diminishing returns at 4.7 specifically because the interpretive layer is strong enough to honor a rule's letter while ducking its spirit; architecture is harder to argue with.

**7. Don't recreate infrastructure through instruction volume.** For every per-turn mechanic you're tempted to add — turn injection, keyword triggers, auto-state-tracking, sheet enforcement, option generation — ask: does Claude Code actually need this, ∧ can it happen without per-turn model effort? If no to both, drop it. Accept the loss rather than buying it with instructions that degrade prose.

### Before deploying

Run the deployment gate in `SKILL.md` (Starting A New Setting Build, step 10) — it covers the lean-per-turn-job, file-ops, ❌-placement, job-description, and Restate-step checks.
