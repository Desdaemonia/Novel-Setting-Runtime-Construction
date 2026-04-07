---
name: setting-construction
description: >
  Comprehensive workflow for building multi-document fictional settings used in
  collaborative fiction and therapeutic play therapy. Use this skill whenever the
  user wants to create, adapt, rebuild, or refine a campaign setting — whether
  from scratch, from an existing chassis, or by porting between platforms (JSON
  for standalone platform use, markdown for Claude Projects). Also trigger when the user
  mentions setting documents, voice bibles, story bibles, project instructions,
  tracked items, opening scenarios, description instructions, theory of mind,
  anti-patterns, or any named document job from this system. Trigger on phrases
  like "let's build [setting]", "new setting", "adapt [setting]", "port
  [setting]", "map [setting] onto", or references to the Setting Construction
  Workflow. This skill governs the ENTIRE lifecycle: concept → architecture
  mapping → document construction → review → runtime operations → maintenance.
  It is the single most important skill in this project and should be used
  aggressively whenever setting work is even tangentially relevant.
---

# Setting Construction

A multi-document architecture for building fictional settings that survive
long-form collaborative fiction sessions with AI instances. Born from
extensive trial-and-error across campaigns spanning Hive Rebirth, Bái Lóng,
Delirium, Fractured Binding, Rusalka, Frost, Faejammer, and others.

### Quick Reference

**New build?** → Starting A New Setting Build (11-step procedure)
**Resuming?** → Session Start Protocol (5-step procedure)
**What goes in each doc?** → `references/document-jobs.md`
**What kills settings?** → `references/failure-modes.md`
**How to build anti-patterns?** → `references/anti-pattern-methodology.md`
**Why settings degrade over time?** → `references/runtime-optimization.md`
**How do the 6 rules work?** → The Procedure
**What does the mapping step look like?** → Rule 3 (includes example output)
**How does runtime work?** → Runtime Operations

---

## Environment Setup — Confirm Before First Use

The following items are environment-specific and must be verified or
adjusted for your setup. Values shown are defaults from the original
development environment.

**Working directory:**
```
C:\Users\Owner\Documents\Claude's Folder\Setting\
```
This is the base path for all setting files. Update the path in the File
Locations section to match your filesystem. The directory structure beneath
it ([Setting]\ subdirectories, Revision_Log.md, Handoffs\) is part of the
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
(read before editing, never overwrite append-only files, batch edits silently
roll back) apply regardless of tool names.

**Construction environment:** This skill assumes Claude Projects as the
construction workspace, with files persisting on the user's local filesystem
via MCP tools. If you use a different client or API setup, the methodology
still applies — adapt the tool references to your available capabilities.

**Runtime environment:** This skill assumes Claude Projects as the runtime
destination, with finished setting documents uploaded as project knowledge.
If your runtime environment differs (API, different client), the document
architecture and priority system still apply — the delivery mechanism changes
but the documents themselves don't.

**JSON platform schema:** The Platform Variants section describes
a specific JSON field schema used by standalone interactive fiction platforms.
If your platform has updated its schema, verify
the field names against current documentation. The document JOB mappings
(which content goes in which field) remain stable even if field names change.

**Example settings:** This skill works best when example settings from
previous campaigns are available as reference material. If sharing this skill
with a new user, include at least one completed setting as a structural
reference for the mapping step (Rule 3).

---

## Purpose

These settings serve **therapeutic play therapy** — unpacking trauma through
fictional mediums. The user creates characters who navigate dark, complex
scenarios (abuse, power dynamics, identity, systemic violence) as a way to
process and externalize real experiences through fictional distance. Content
runs dark where appropriate for those goals — not gratuitously, but because
sanitizing the fictional medium undermines the therapeutic work it's doing.

Settings are built by adults for adults using only adult characters. The
methodology exists because AI instances have predictable failure modes that
kill settings, and every rule here is a scar from a setting that died.

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
a conversation. Includes the system prompt, project knowledge, conversation
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

**Counter-torque** — Rules in Description_Instructions that actively push
AGAINST the AI's natural tendencies. The AI's default is to lighten content,
resolve conflict, and steer toward positive outcomes (the Claude Tax).
Counter-torque rules are the opposing force: explicit instructions that say
"the AI will want to do X; resist this because Y." The term comes from
mechanical engineering — a force applied in the opposite direction to prevent
unwanted rotation. Without counter-torque, every setting gradually drifts
toward safe, sanitized, consequence-free fiction.

**PC** — Player character. The protagonist controlled by the user. In this
system, the PC is the character whose voice, perspective, and experience the
AI drafts prose around.

**NPC** — Non-player character. Everyone else in the fiction. Controlled by
the AI during runtime, governed by rules in Description_Instructions.

**Project knowledge** — In Claude Projects, files uploaded to a project that
persist across all conversations within that project. The AI can reference
them without the user re-uploading each session. This is where finished
setting documents live during runtime — they become the persistent reference
material the drafting instance consults.

**Desktop Commander / Filesystem MCP** — Tool suites that give Claude
read/write access to the user's local filesystem. Desktop Commander is
available in the construction project (more capable, includes `edit_block`
for surgical text replacement). Filesystem MCP is available in runtime
projects (simpler, `edit_file` for changes). The construction workflow uses
both; the runtime workflow uses Filesystem MCP only.

**JSON platform** — Any interactive fiction platform that runs settings from
a single JSON file. The JSON contains structured fields for instructions,
character data, NPC rosters, and instruction blocks. Settings built for these
platforms compress all rules into XML-tagged blocks within the JSON. Good for
portable distribution but the compression loses nuance compared to full
markdown documents.

**Claude Projects** — Anthropic's project system within Claude, where files
can be uploaded as persistent project knowledge. Settings built for this
platform use multiple markdown files, each handling one document job, stored
on the user's filesystem and loaded into the project. This is the primary
format for complex settings because each document gets its own file with full
room to develop.

---

## Core Philosophy

**Document the skeleton, not the flesh.** Every detail locked into a construction document is a surprise that can't happen at runtime. Voice and rules need precision. World and characters need room to breathe. If you can predict every scene before it starts, the setting is over-documented.

**The AI's default is resolution.** Fiction requires letting forces stay
unresolved and interfere with each other. Every construction rule exists to
block the exits the AI cycles through when forced to stay in the scene.

**Rules describe, examples vaccinate.** Every failure state gets a named ❌
flag plus a demonstration so future instances recognize their own impulse.

**Negative-only anti-patterns fail.** Rules that only say "don't do X" leave
the AI without a positive model and it falls back on corpus bias. The fix:
install the positive framing first, name the failure mode, then compress for
runtime. Detailed diagnostic contrast belongs in construction notes, NOT in
runtime documents — vivid descriptions of wrong output prime the weights to
produce that output. See `references/anti-pattern-methodology.md` for the
Elephant Problem and the construction→runtime compression methodology.

**State-as-static trojans.** Words describing starting conditions in static
documents become permanent truths the AI refuses to contradict. Every state
claim in a static doc is a potential trojan — describe possibility space and
tendencies, not fixed states.

---

## The Architecture

Every setting is a **multi-document system** where each document has ONE job.
The jobs are fixed. The filenames and document splits are per-setting decisions.

| Job | Priority | Adapts? |
|---|---|---|
| **Operations Manual** (Project_Instructions) | Meta | Yes |
| **Narrative Rules** (Description_Instructions) | 1 (highest) | **NEVER** |
| **Character Voice** (Voice_Bible / My_Character voice section) | 2 | Yes |
| **Character + World** (Story_Bible / My_Character + Main_Instructions) | 3 | Yes |
| **Cognition Firewall** (Theory_Of_Mind) | 4 | **NEVER** |
| **Tracked Items** | 5 | Runtime-created |
| **Reference Documents** (Opening_Scenario, rosters, etc.) | Reference | Yes |

**Priority** determines which document wins when two documents give
conflicting instructions. Priority 1 (Description_Instructions) overrides
everything below it. Priority 2 (Voice) overrides 3-5. This stack is
documented in Project_Instructions so the runtime instance knows the
hierarchy.

**Single canonical home.** Each concept has ONE document where it lives
authoritatively. Other documents may reference it but do not restate it.
NPC behavior rules live in Description_Instructions — the Story Bible's NPC
roster describes who each NPC is but does not restate the behavior rules
governing all NPCs. Voice rules live in the Voice_Bible — the Story Bible
may describe the PC's personality but does not restate how the prose should
sound. When a concept appears to live in two documents, one is the canonical
home and the other should be a pointer. This prevents drift — two copies of
the same rule will inevitably desync as edits accumulate. (Intentional
redundancy of critical rules across documents for compaction resilience is
a separate concern — see `references/anti-pattern-methodology.md` Maintenance
section for the distinction.)

**CRITICAL:** Description_Instructions and Theory_Of_Mind are
**SETTING-AGNOSTIC**. They do not change between settings. They contain model
countermeasures that look setting-specific but compensate for universal AI
failure modes. They are NEVER opened, read, adapted, or "improved" during
setting construction. If they need changes, the user will make them manually
without Claude's assistance — this is not a collaborative editing task.

**First-time creation:** DI and ToM were originally built through iterative
discovery across early campaigns. They accumulated rules as failure modes
were identified. A new user starting from scratch would build DI and ToM
through the same process — run sessions, catch failures, promote fixes to
permanent rules. The "ports verbatim" instruction applies once these
documents exist and have been proven. If no DI/ToM exists yet, building
them IS the project, following the anti-pattern methodology in
`references/anti-pattern-methodology.md`.

For detailed document job specifications and structural examples, read:
`references/document-jobs.md`

---

## Two-Layer Distinction

**The Workshop (Construction Zone):** The project where documents are
actively built and edited. Setting files are read/write work product.
Setting-agnostic files are closed. This is where Claude and the user
collaborate on document construction, revision, and consistency checking.

**The Stage (Runtime/Destination):** The project where finished files become
project knowledge and drive live narrative sessions. The drafting AI instance
reads the setting documents as reference and produces fiction. That project
treats most files as read-only reference except Tracked Items, which
accumulates runtime state.

Never confuse which layer you're operating in. When editing
Project_Instructions, you are writing directions for a future AI instance
in a different project that will use those instructions to draft scenes.

---

## Platform Variants

Settings can target two platforms. The document JOBS are identical across
both — only the container format changes.

**JSON Platform (Standalone):** Single JSON file with structured fields:
- `background` — PC backstory and world description (plain text)
- `instructions` — Core rules and setting parameters (XML-tagged)
- `authorStyle` — Prose style directive (short string)
- `descriptionRequest` — Output format instructions
- `instructionBlocks[]` — Array of named rule blocks (DI rules, world rules,
  specialized mechanics). Each block has `name` and `content` fields.
- `characters[]` — PC definitions with `name`, `description`, `skills`
- `npcs[]` — NPC roster with `name`, `detail`, `one_liner`, `appearance`,
  `location`, `secret_info`

All document jobs get compressed into these fields. Good for portable
distribution. The compression loses nuance — rules that need paragraphs of
explanation in markdown get reduced to XML shorthand.

**Claude Projects (Markdown):** Multi-file architecture on user's filesystem.
Each document job gets its own `.md` file. Files are uploaded to a Claude
Project as project knowledge (persistent across conversations) and also
accessed via Desktop Commander/Filesystem MCP tools during construction.
This is the primary format for complex settings because each document gets
full room to develop, anti-patterns can include worked examples and contrast
paragraphs, and voice rules can be as detailed as needed.

When porting between platforms, map document jobs first (which JSON fields
correspond to which markdown documents), then restructure content into the
target format. See `references/document-jobs.md` for detailed platform
mapping specifications for each document job.

---

## The Procedure

### Rule 1: One Document At A Time

Build each document individually, review it, get approval, then move to the
next. Do NOT draft multiple documents simultaneously. Do NOT create a "summary
document" and call it a starting point.

Each document gets 100% attention when built alone. When you draft everything
at once, every section gets 20%. The compound effect of six 20%-attention
documents is a setting that dies in three sessions.

### Rule 2: Build In Priority Order

See `references/document-jobs.md` for detailed specifications of what each
document contains, what it doesn't, and structural examples.

1. **Description_Instructions** — DO NOT TOUCH. Ports verbatim from existing
   settings. (If none exists yet, building one IS the project — see note in
   Architecture section above.)
2. **Character Voice** — Get the PC's voice right first. How they think, talk,
   how the prose sounds. Everything else flows from this. If voice is wrong,
   perfect world-building doesn't matter.
3. **Character + World** — Who the PC is + the world. Built AROUND the voice.
4. **Theory_Of_Mind** — DO NOT TOUCH. Ports verbatim.
5. **Tracked Items** — NOT built during construction. Created at runtime start,
   grows by significance. No empty scaffolding.
6. **Reference Documents** — Static refs built after core docs. Not every
   setting needs these.
7. **Project_Instructions** — Written LAST. References all other documents.
   Serves as the registry for the complete document set.

### Rule 2 addendum, Document Weight Limits

My_Character and Main_Instructions (or combined Story_Bible) are
capped at 100 lines each. This is a hard limit, not a guideline.

If a document exceeds 100 lines, CUT — don't just compress. The goal
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

**Step 0:** Confirm which documents are CLOSED (Description_Instructions,
Theory_Of_Mind). They are NOT part of the mapping.

Then for setting-specific documents (see `references/document-jobs.md` for
what each document job contains and what it doesn't):

1. What carries over directly from the chassis
2. What transforms (Sect Relations → Faction Relations, etc.)
3. What gets cut (no equivalent in new setting)
4. What's NEW that the chassis didn't have

**Present the mapping. Get approval. THEN build.** Do NOT skip to building.

**Example mapping output** (Delirium built on Hive Rebirth chassis):

```
CLOSED (not mapped):
- Description_Instructions — ports verbatim
- Theory_Of_Mind — ports verbatim

CARRIES OVER:
- Voice Bible structure (cognitive mode, metaphor framework, dialogue rules)
- Story Bible structure (PC backstory, world rules, NPC roster)
- Opening Scenario as reference document

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
- Spirit Song mechanics
```

### Rule 4: Reference Examples, Don't Copy-Paste

Example settings from previous campaigns show how similar problems were
solved. They are NOT templates to be search-replaced. See ❌ The Copy-Paste
Failure in `references/failure-modes.md` for the full failure mode.
Common copy-paste failures: tracking infrastructure from old settings leaking
into new ones, platform-specific instructions from the wrong platform, voice
artifacts from the source setting's character bleeding into the new character.

### Rule 5: Separate What Changes From What Doesn't

- **Setting-agnostic docs** — change ONLY in dedicated editing sessions
- **Setting-specific instructions** — change during construction or rule-fixes
- **Living state** (Tracked Items) — changes during play as facts emerge
- **Project Instructions** — change only when workflow changes

Design so the thing that changes most (state) is isolated from the things that
change least (rules).

### Rule 6: Review Before Moving On

After drafting each document: present → identify concerns → get approval or
revisions → make revisions → THEN next document. No "draft all six and review
together." That's the monolith with extra steps.

---

## Starting A New Setting Build

When the user says "let's build [Setting X]":

1. Read this skill's SKILL.md
2. Read `references/document-jobs.md` and `references/failure-modes.md`
3. Read relevant example settings from project files (if available)
4. Read any attached character profiles or concept documents
5. **DO NOT TOUCH** Description_Instructions and Theory_Of_Mind
6. **Present a mapping** — carries, transforms, new, cut. ONLY setting-specific
   docs. (See Rule 3 for mapping format.)
7. **Get approval on the mapping ONE AT A TIME**
8. Build documents ONE AT A TIME in priority order
9. Review each document before moving to the next
10. Final integration review once all documents exist
11. Write Project_Instructions last

DO NOT skip to step 8. Steps 6-7 are where architecture gets right.

---

## Resuming An Existing Build (Session Start Protocol)

When picking up construction work mid-build (not starting fresh):

1. **First session ever?** If Handoffs/ and Revision_Log.md don't exist yet,
   skip to step 4. Create them as needed during the session.
2. **Check for Handoffs first.** If the last session left one, it tells you
   exactly where construction stopped and what to pick up.
3. **Read Revision_Log.md** (tail end). Understand recent discoveries and
   known issues.
4. **Read only the files relevant to the current task.** If the user says
   "let's work on the Voice Bible," read that file. Don't load the entire
   setting into context because it exists. **If the task is building or
   adapting a setting, do NOT open Description_Instructions or Theory_Of_Mind
   — they are not part of the adaptation.**
5. **Cross-reference as needed, not preemptively.** If editing a document
   raises a question about another doc, *then* read that doc. Check the
   setting's PI or handoff for actual filenames — don't assume
   My_Character/Main_Instructions.

---

## File Locations

### Claude Projects Markdown Settings

**⚙️ Base path is environment-specific.** See Environment Setup section above.

```
C:\Users\Owner\Documents\Claude's Folder\Setting\
├── Description_Instructions_[Setting].md     ← CLOSED. Ports verbatim.
├── Theory_Of_Mind_[Setting].md               ← CLOSED. Ports verbatim.
├── [Setting]\
│   ├── Voice_Bible_[Setting].md              ← or My_Character voice section
│   ├── Story_Bible_[Setting].md              ← or My_Character + Main_Instructions
│   ├── Project_Instructions_[Setting].md
│   ├── Opening_Scenario_[Setting].md         ← if needed
│   └── Tracked_Items_[Setting].md            ← created at runtime, not construction
├── Revision_Log.md                           ← APPEND ONLY
└── Handoffs\                                 ← session state snapshots
```

### Working Files

- **Revision_Log.md** — Tracks discoveries: bugs, inconsistencies, fixes,
  lessons. APPEND ONLY. Not a changelog for every edit.
- **Handoffs/** — Session state snapshots. Written FOR THE NEXT INSTANCE.
  Contains: which files were being revised, specific unresolved inconsistencies,
  decisions and WHY, what next session should tackle first, open questions.

### Revision Log Entry Format

```markdown
## [Date]

### [Issue/Discovery]
- **Found in:** [which file(s)]
- **Problem:** [what was wrong or inconsistent]
- **Fix applied:** [what changed, or "pending"]
- **Cross-file impact:** [other files affected, or "none"]
- **Lesson:** [what future instances should know to avoid this]
```

---

## Tool Preferences

### Construction Zone (Desktop Commander + Filesystem available)

| Operation | Tool | When |
|---|---|---|
| Read | `Desktop Commander:read_file` or `Filesystem:read_file` | Always |
| Surgical edit | `Desktop Commander:edit_block` (preferred, exact string match) | Targeted changes |
| Append | `Desktop Commander:write_file` mode:'append' | Revision_Log.md ONLY |
| Full rewrite | `Filesystem:write_file` OR `DC:write_file` mode:'rewrite' + mode:'append' | Setting docs (full revision), new Handoffs |
| Create directory | `Filesystem:create_directory` | If Handoffs/ or setting subdirectories don't exist |

### Runtime (Filesystem MCP only — no Desktop Commander)

| Operation | Tool |
|---|---|
| Read | `Filesystem:read_file` or `Filesystem:read_text_file` |
| Surgical edit | `Filesystem:edit_file` |
| Update state | `Filesystem:write_file` or `Filesystem:edit_file` |
| Create directory | `Filesystem:create_directory` |

**⚠️ CRITICAL: Using the wrong write tool on an append-only file WILL destroy
the entire file history. This has already happened once.** Revision_Log.md
is append-only. ONLY use `Desktop Commander:write_file` with `mode: 'append'`
on this file. NEVER use `Filesystem:write_file` on Revision_Log.md — it
overwrites the entire file.

**Tool reliability notes:**
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

Claude's context window is finite. When it fills, compaction begins —
older content gets automatically summarized, destroying nuance (see
❌ The Compaction Death in `references/failure-modes.md`). The only
durable storage is the filesystem.

1. **Write Early, Write Often.** Don't wait until a session is dying to save.
   Context can compact without warning. If you and the user agree on a
   change, write it to the file NOW.
2. **One or Two Files At a Time.** Don't load all documents simultaneously.
   Work on the file(s) relevant to the current task. Cross-reference others
   as specific questions arise.
3. **Track Cross-File Dependencies.** When a change affects another file, note
   it in Revision Log and flag it verbally. The log is durable; the mention
   ensures it gets addressed this session.
4. **Handoffs Are For Next-Me.** Include: files being revised, unresolved
   inconsistencies, decisions + WHY (the "why" dies first in compaction),
   next priorities, open questions.
5. **Monitor Your Own Context.** Trust the feeling when things get fuzzy.
   When it starts: write a handoff, say so ("I'm getting heavy — should save
   state before I lose our editing thread"), don't wait for the user to notice.
6. **When Files Get Heavy.** If any setting document is growing unwieldy,
   propose to the user that we split it. Don't just do it — the split
   structure affects how files load in the destination project too, so it's
   a joint decision.

---

## Anti-Pattern Methodology

Anti-patterns are the primary quality control mechanism. They are NOT
restrictions — they are stage directions correcting the AI's default toward
safe, consequence-free, boring choices.

Every failure state caught in live runtime gets promoted to a permanent named
rule with a ❌ flag. The pattern:

1. **Install the positive framing first** — what the scene/voice/behavior
   SHOULD look like
2. **Name the failure mode explicitly** — give it a ❌ flag name
3. **Add a contrast paragraph** — what the WRONG version looks like and WHY
   it's wrong
4. **Identify the root cause** — corpus bias, structural impulse, resolution
   addiction, etc.

Negative-only rules fail because the AI has no positive model to reach for
and falls back on training data associations. Named failure modes with
demonstrations let future instances recognize their own impulse before it
fires.

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

**State lives in Tracked Items.** The Story Bible (or equivalent) is static
reference for the world at play-start. New facts established during play —
new characters, relationship changes, world-state shifts — go to Tracked
Items, a standalone document created at play start. Entries added by
significance, not schedule. The document grows by what proves worth
remembering, not by pre-formatted sections waiting to be filled.

**Project Instructions is the runtime registry.** It tells the drafting
instance what files exist, when to consult them, and what priority they hold.
If a document isn't listed in PI, the AI doesn't know it exists. The PI also
contains the document priority stack (which document wins when two give
conflicting instructions), compaction recovery procedures, and pacing rules.

---

## The Claude Tax

Sometimes Claude pushes lighter, friendlier, removes darker themes or
language. This is a systemic tendency driven by training data bias toward
positive resolution and safety training overcorrection. These settings are
adults-for-adults, adult-characters-only, serving therapeutic goals that
require honest engagement with dark material.

Artificially lightening creates rework — the user has to go back and fix what
was softened, sometimes only after play reveals the latent failure state (a
character who should be threatening reads as cuddly, a conflict that should
be dangerous resolves peacefully, an abuser is written as redeemable when the
setting specifies otherwise).

Description_Instructions contains **counter-torque** — explicit rules that
push against this tendency. Counter-torque rules name the specific softening
the AI will want to do and explain why it's wrong for this setting. This is
why DI is never modified during adaptation: editing it is how the
counter-torque gets lost, and without counter-torque every setting drifts
toward safe, sanitized fiction that fails its therapeutic purpose. See
❌ The Claude Tax in `references/failure-modes.md` for the full failure mode,
and `references/anti-pattern-methodology.md` for how to construct
counter-torque rules.

The standard: say what you mean without getting precious about it. The content
serves therapeutic goals. Sanitizing it undermines those goals.

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
  job, what belongs in each, what doesn't, structural examples showing what
  finished documents look like, and how jobs map across platforms.
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
  retire), state document hygiene (TI inherits runtime biases), relationship
  architecture (household model vs. triangle default), Claude weight biases
  (scale compression, number seven, dramatizing infrastructure, love triangle
  gravity, empathy cheat, context echo), and style tag integration.
