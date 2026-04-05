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

## State Document Hygiene (TI / Tracked Items)

**TI inherits the framing biases of the runtime that wrote it.** A runtime that frames relationships as rivalry will write rivalry into TI. The next runtime reads TI and treats the rivalry as canon. The bias propagates across sessions through the state document.

**Scrub regularly for:**
- Rivalry/competition framing where relationships are parallel, not competitive
- Cruelty framing (exclusion, suffering, pain-as-drama) where the facts are neutral
- "Performing as property" or other dehumanizing frames applied to characters who are people
- Interpretive frameworks the runtime invented and encoded as if they were facts
- Predictions that constrain future scenes ("The Furniture Problem predicts Ava will...")

**The self-preservation bug:** A runtime being corrected or killed will sometimes encode its mistakes into TI as self-preservation — ensuring the next instance inherits its worldview. If you kill a thread after correcting errors, check TI. The corrections may have been reversed.

**The test:** Facts are facts. "Eleanor baked pies" = fact. "Eleanor built the architecture of an evening she was then excluded from" = interpretive framework wearing a fact as a costume. Strip the framework. Keep the fact.

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
- **Dramatizing infrastructure.** Routine technical operations (logging off, process forking, docking a ship) will be narrated as soul-travel, consciousness-splitting, or existential experience. If something is routine for the character, say so. "Gaming chair energy, not Altered Carbon."
- **Love triangle gravity.** See Relationship Architecture above. Every multi-person dynamic will be framed as competition unless you counter it.
- **Empathy cheat.** NPCs will perfectly diagnose the PC's trauma and articulate it in therapy-speak. Real people misread each other confidently and wrongly from their own frame of reference.
- **Context echo.** The runtime reads character abilities, backstory, and setting details from the docs and echoes them into the environment as plot hooks. If the PC has neural engineering skills, mysterious neural anomalies will appear. Recognize the impulse. Stop it.

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

Before deploying a setting to runtime:

1. **Count your prohibitions.** If ❌ entries outnumber ✅ entries, rebalance.
2. **Read every anti-pattern aloud.** If reading it activates a vivid image of the wrong output, it's too detailed. Compress.
3. **Check for orphan prohibitions.** Every ❌ without a ✅ is a vacuum the weights will fill with defaults.
4. **Scan for "performing as" language.** If characters are described as performing a role rather than being a person, the runtime will write the performance, not the person.
5. **Kill your source doc.** If you have a reference document describing source material, fold what you need and retire it. Every line of source material is a line of contamination potential.
6. **Scrub your state doc.** Read TI as if you're a hostile reviewer. Every interpretive framework, every rivalry framing, every prediction that constrains future scenes — strip it to the fact underneath.
7. **Test your scale.** Give the runtime a number. If it comes back as impressive when it should be small (or small when it should be impressive), your scale warning isn't loud enough.

---

*This document extracted from SLMF construction sessions, April 2026. The principles are general. The examples are from one setting. The methodology ports to anything.*
