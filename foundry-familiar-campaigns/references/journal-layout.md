# Campaign journal layout

One predictable layout, so any playing AI (or human GM) knows where to look. Create the folder first, because Familiar can't move journals between folders later.

```text
📁 <Title> (GM)                          ← create-folder type JournalEntry
   📕 <Title>: Start Here                 small, read in full every session
        0 Runner Guide                    how to run THIS campaign (stable)
        1 Pickup                          the one current checkpoint (overwritten)
        2 Table Profile                   who plays what, rules, limits, presentation
        3 Adaptations                     approved changes from the source
        4 Trackers                        codewords, clocks, rewards owed, custody
        5 Session Log                     what actually happened, newest first
   📗 <Title>: Source                     original text, one page per section
   📘 <Title>: Prep <segment>             runnable notes for the next stretch
   📙 <Title>: Handouts                   player-safe; the GM shares these by hand
```

Rules of thumb:

- **Start Here stays small**, under about 20,000 characters in total, because the runner reads it with one `get-journal` call. Move old Session Log entries to an archive journal (`<Title>: Session Log archive`) once it grows.
- **Exactly one current Pickup.** Overwrite it at closeout. History goes in the Session Log, never in a second "current" page.
- **Names are lookups.** Keep journal names stable and ASCII-simple. Renaming the Start Here journal breaks the Table Rules pointer. If you rename, update the rules too.
- **Record IDs.** After creating, write the journal and page IDs into the Runner Guide's *Where things are* section and into the Table Rules.
- **Spoiler-safe names.** Document, page, scene and actor names may be seen by players (sidebar, chat speaker names, tokens). "Prep: The Ice Cave" is fine. "Prep: Thalgar's Betrayal" is not.

## Page templates

Write pages in simple HTML. Each template below is shown as plain text; wrap the sections in `<h2>`/`<p>`/`<ul>`. **No `<table>` in pages the runner reads:** `get-journal` and `get-journal-page` strip HTML to plain text, and table cells run together without separators. Use one paragraph or list item per row instead. **Put a line break after every block tag** (`</h2>
<p>…`). Plain-text reads glue a heading to the next paragraph otherwise ("PeopleChief Ghudvil…"). **Reads return plain text, not your HTML**, so to update a long page, rebuild it from the HTML you wrote (keep a copy) rather than from what `get-journal` returns. Replace every `<…>` placeholder. Delete sections that don't apply rather than leaving them empty.

### 0 Runner Guide

This is the procedure the playing AI follows. It doesn't have this skill, so this page *is* its skill for this campaign.

```text
RUNNER GUIDE: <Title>
You are the game master for <Title>, a <system/edition> <format: gamebook / duet / party> adventure, for <players>.

AT THE START OF EVERY SESSION (and after a break, a new chat, or "let's continue")
1. Read this journal in full (get-journal "<Title>: Start Here").
2. Read the Source page(s) the Pickup names (get-journal-page with identifier "<Title>: Source" and the pageId from WHERE THINGS ARE), and the matching Prep page. Look journals up by exact name; IDs are a backup.
3. Check that live Foundry state matches the Pickup: active scene, token positions, HP and resources. Only chat messages posted AFTER the Pickup was last updated can override it (play a closeout missed): bring the Pickup up to date and tell the GM. Older chat never overrides it. If the GM or preparing assistant changed the Pickup, that's deliberate: follow it, and flag anything odd instead of reverting it.
4. Give a short recap of what the characters know, then stop at the pending decision. A greeting is not an action: don't advance time or act for anyone.
5. Combat or a new location: switch to the scene named in Prep and place tokens at the canvas positions written there (Familiar can only place tokens on the active scene).

RUNNING PLAY
- Original text wins. Use the Source page's wording for read-aloud text, choices, DCs, numbers, times and rewards. Summaries in Prep are aids. Changes listed in Adaptations override the source; nothing else does.
- When the party reaches a new section or entry, read its Source page first. Don't narrate from memory.
- If a fact you need isn't written down, read the relevant page. If it truly isn't defined, invent only harmless colour and keep it consistent. Never invent clues, solutions, rewards, NPC knowledge or difficulty changes. Ask the GM instead.
- NPCs know only what their notes say they know.
- Never reveal unvisited content, DCs, monster stats or upcoming events. <hint policy from Table Profile>.
- The player controls <character(s)>. Never write their words or decisions, even when the source scripts them ("you say..."): stop and ask.
- Resolve each roll once, with real dice/tools. Update HP, items and Trackers when things change.
- Gear bought or found goes ON the character's sheet: search-compendium then import-compendium-to-character, or create-item on that character. Never create a new actor for an item, and never delete actors.
- Post only in-world narration, NPC dialogue and direct answers. Never narrate your own tool use ("let me read...").

CLOSEOUT (when the player says they're stopping)
1. Finish the current beat and don't start a new one. Don't move anyone or advance time.
2. Read the Source page for the next likely section(s).
3. Overwrite "1 Pickup" in ONE write (update-journal-page), as HTML with one <p> per heading: Updated; Last session (3-6 lines of what actually happened: entries reached, choices, rolls that mattered, damage, purchases, codewords); Position; Foundry (active scene); Party (HP, resources, gold, key items); Codewords/flags; Pending decision; Read next; Warnings.
4. Save 3-8 durable facts with save-campaign-memories (only things that stay true).
5. Read the Pickup back (get-journal-page) to confirm it saved. Then say goodbye with a one-line, spoiler-free "next time".
(The preparing assistant later moves "Last session" into the Session Log and updates Trackers. Keep the runner's closeout to one write. In the live test, runners reliably made one page write and skipped the rest.)

WHERE THINGS ARE
Start Here: <journal id>. Pages: Runner Guide <id>, Pickup <id>, Table Profile <id>, Adaptations <id>, Trackers <id>, Session Log <id>
Source: <journal id>; page index: <name → id, one per line>
Prep: <journal id>
Scenes: <name → id>.  Actors: <name → id>
```

### 1 Pickup

```text
PICKUP (current; overwritten each closeout)   Updated: <date>, after session <n>
Last session: <3-6 lines, facts only; the preparing assistant files this into the Session Log>
Position: <Source page + entry/section>; fictional <day/time>; place <…>
Foundry: active scene <name/id>; tokens <where>; scenes staged next: <names>
Party: <PC: HP x/y, resources, conditions>; <sidekick/companions>
Carrying/owed: <key items, money, rewards promised but not paid>
Pending decision: <exactly what the player must decide next, as they know it>
Open threads: <short list>
Read next: Source <page names>; Prep <page name>
Warnings: <unresolved contradictions or anything the GM must fix; "none">
```

Before the first session, the Pickup says "Not started" and points at the opening section.

### 2 Table Profile

```text
Players → characters: <user> plays <actor (id)>; sidekick <actor> controlled by <player/GM>
Rules: <system, edition>; house rules: <…>; Familiar combat automation: <on/off/not supported>
Audience and spoilers: <who must not see what>; hint level: <none / on request / helpful>
Presentation: <maps+tokens / theatre of the mind>; music <yes/no>; chat record <how>
Content limits: <lines and veils, tone>
Pace and length: <session length, how many sessions planned>
```

### 3 Adaptations

The ledger list from [Source intake](source-intake.md#adaptations-ledger), one numbered paragraph per change. It says "None yet" if there are none.

### 4 Trackers

```text
Codewords/flags: <name: gained in entry N, effect>   (gamebooks)
Clocks the player can see: <name: x/y, what advances it>
Rewards owed / promised: <who, what, when, status: promised/earned/paid>
Custody: <important items: who has them, where>
(Prepared events, clocks the player mustn't see, and future codewords belong in the Prep journal, not here. The runner reads Start Here in full every session.)
```

### 5 Session Log

```text
Session <n>, <date>: <Source entries/pages covered>
- <what happened, in order, facts only; decisions, key rolls, outcomes>
- Gained: <items, money, codewords, XP>. Lost/spent: <…>
- Changes from source this session: <none / see Adaptations #n>
```

## Prep page skeleton

One page per scene, location or entry group. Keep the Source page reference in its first line.

```text
<Place / entry range>   Source: <page name>, pp. <n>
Situation: what is going on here and why, independent of the PCs
Read-aloud: "<the source's boxed text, verbatim, or 'see Source'>"
Place: layout and routes; things to examine/use; what searching finds
People: <NPC>: wants / knows / doesn't know / will share / voice
Choices: the options the source offers, plus sensible others, and where each leads
Checks: <skill, DC, success/failure>, verbatim from source
Encounter: <creatures + actor ids>; tactics; terrain; rules to check (see session-prep)
Rewards: exact source rewards; codewords
Leads to: <next entries/sections>
```

## Repair, don't duplicate

If the layout already exists, update pages in place (`update-journal-page` replaces the whole page, so read, edit, write). If a journal ended up in the wrong folder, it can stay; just record its ID. If two "current" pickups exist, merge them into page 1 and move the older one into the Session Log as history.
