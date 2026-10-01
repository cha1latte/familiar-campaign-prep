# Worked example: installing *Frozen Offerings*

A real run of this skill, October 2026: Foundry 14.367, dnd5e 6.0.1, Familiar 2.25.0, in a throwaway world. The request was *"find me a free one-player D&D adventure and set it up."* The adventure is Wizards of the Coast's free solo gamebook from *Dragon+* 34, licensed for personal use. None of its text is reproduced here; this page records **what was done and what was caught**.

## 1. Orient and find

- The world had one player account with a level 7 fighter, dnd5e, and no journals or scenes.
- Search for "free solo D&D 5e adventure" surfaced the publisher's own free PDF (preferred) next to file-sharing mirrors (ignored).
- Fit check: solo gamebook ✔, levels 7–10 ✔ (the PC is 7: "very tough", per the book), includes a sidekick and three battle maps.

## 2. Source intake: what naive extraction got wrong

| Check | Finding | Fix |
|---|---|---|
| Plain `extract_text()` | Two columns interleaved line by line; headings doubled ("FFRROOZZEENN") | Crop each page into left/right columns; `dedupe_chars()` |
| Fonts | Entry headings, codewords and "go to entry" lines each use a distinct font | Used the fonts to tag `<h3>`, `<strong>` and `<strong><em>`, so nothing was guessed |
| Word-level comparison | 16,229 / 16,229 words present; all 76 entries, in order | n/a |
| **Page image, p. 2–3** | A *Playtesters* sidebar was spliced mid-sentence into the backstory, yet the word count still passed | Sidebars became separate blocks; the paragraph rejoined |
| **Page image, p. 22** | Bullet points split at line wraps; an "Actions" heading glued onto a trait | Hanging-indent detection; heading font added |
| Coverage re-run | Caught a bug in my own heading fix (a control character replaced "9") | Fixed before upload |
| Source quirks | Typo "extra moment"; heading "9.False Friends" | Typo kept verbatim, heading spacing normalised; both noted in *00 About this source* |
| Apparent contradiction | Dragon named Angnath (she) in entries, Xakkrath (he) in combat sheets | **Not an error.** Entry 10 reveals Xakkrath is Angnath's son. Recorded as a GM-only spoiler, not "fixed" |

Lesson: **word counts prove nothing about layout**. Three of the five real problems were visible only on the page image.

Result: 13 Source pages (3–13k characters each), plus the three battle maps pulled from the PDF at full resolution (1504 px, 25×25 squares, resized to 1500 px for 60 px squares).

## 3. Prep from the source

- Walked the entry links from entry 1 to find session 1's reachable set: backstory → shopping → the voyage (two branches by codeword) → an optional ambush → landing → the chief's longhouse. Session 1 stops at the longhouse choice.
- Four prep pages, NPC knowledge cards for Thalgar, Rhia, Utrirr and Chief Ghudvil, and a danger check for the one fight.
- **Fact trace:** a script pulled every DC, damage roll, price, codeword and d20 band out of the prep and searched the source for each. All matched except "Dex save", which I'd abbreviated. Changed to the book's exact "Dexterity saving throw".
- Adaptations recorded before play: (1) the AI GM rolls for monsters and random events; (2) the *Sidekicks Essentials* supplement isn't used, so Thalgar uses his printed block; (3) the book scripts the PC's lines ("you say…"), and the AI GM must offer them, not speak for the player; (4) printed 2014-era stat blocks kept as-is.
- Table Profile warning: the player's character was built without the adventure's starting gold and uncommon magic item. That's flagged for the GM, not silently added.

## 4. Build in Foundry (through Familiar's tools)

| Step | What happened | Lesson now in the skill |
|---|---|---|
| Folders | Journals, Actors and Scenes folders created first | Familiar can't move journals afterwards, and can't put scenes in folders at all |
| Source journal | 4 pages pushed by hand through `create-journal-page`, then 10 by script through a separate Assistant-GM login, with the Familiar module blocked in that tab. **14/14 pages matched the extraction exactly** on read-back | Push long text by script when you can; always read back and diff |
| Thalgar and the Deep Scion | Built from the printed blocks. A scripted read-back found **13 mismatches**: skill bonuses silently dropped, skills computed from DEX, weapons with no attack activity, a compendium handaxe with the wrong die. All fixed and re-checked until every number matched the book | Read every computed number back against the book; the dnd5e 6 traps are listed in Scene production |
| Scenes | The book's three battle maps, inactive, out of navigation, globally lit (the book gives no darkness rules) | n/a |
| Token placement | `create-tokens` only works on the active scene, so positions were planned and checked on a client-side preview instead (all four landed on the boat) | Write pixel positions into Prep; the runner places tokens on arrival |
| Start Here | Read back with `get-journal`: the HTML **Adaptations table came back as run-together text** | No `<table>` in runner-read pages |
| Table Rules | ~1,240 characters written, read back intact via `get-world-info` | n/a |

## 5. The handoff, tested with the real playing AI

The playing AI was Familiar's **table chat**, with the player typing `@familiar …` in Foundry chat from a separate player login. No IDs or special prompt were given.

| Attempt | Result | Fix |
|---|---|---|
| 1 | "No character is linked to your player yet" | Created the player account with `create-user { character }` and set it as Solo Player |
| 2 | "Your character has no token on the active scene" | Opening backdrop scene (the book's cover art), activated, with Kestrel's token |
| 3–5 | The AI **called `get-journal` correctly**, then returned empty text every turn | Model issue (Gemini 3.8 Flash). With the GM's OK, switched to DeepSeek V4 Flash for the test, then restored |
| 6 "hi! let's play" | Accurate recap from the Pickup; raised the Pickup's warning about missing starting gear; waited | ✔ |
| 7 gear + "let's start" | Rolled 1d10×25 for real, added the cloak and 650 gp, **read the Source pages**, narrated the backstory faithfully, and stopped exactly where the book has the PC speak | A mistyped journal ID failed once (it recovered by searching), so the rules now say "look up by name". Tool chatter leaked into chat, so the rules now say "never narrate tool use" |
| 8 talk to Thalgar | Every NPC line matched the book, but **two of Kestrel's scripted lines were narrated for her** | The rules now say "never write the PC's words, even when the book scripts them" |
| 9 "let's stop for tonight" | Read the Source, rewrote the Pickup accurately, saved 5 good memories, **skipped the Session Log, Trackers and read-back** | Closeout redesigned to one Pickup write with a *Last session* block; the preparing assistant files the log |
| 10 "let's continue" | `get-journal` then `get-character ×2`, "Everything matches", accurate recap, stopped at the decision | ✔ |
| 11–12 play, then stop again | Off-script choice (refusing payment) handled fairly. New one-write closeout: **Pickup with Last session written correctly** after reading the Source; memories saved; the read-back was still skipped. Saved as plain text, so it reads as one block in Foundry | Runner Guide now asks for HTML, one `<p>` per heading |

## Honest scorecard of the run

- **Verified:** source import (byte-exact), stat blocks (number-exact), scenes rendered, Table Rules installed, runner retrieval (tool receipts), faithful narration, resume, one-write closeout.
- **Partly:** player agency (one slip before the rules fix); per-entry Source re-reads (the runner sometimes relied on earlier reads); closeout read-back.
- **Not tested:** combat on the staged scenes, multi-player tables, outside MCP assistants as the runner, other game systems.


## 6. Second round: the "not proven yet" list

| Test | Result | Lesson now in the skill |
|---|---|---|
| **Outside AI as the playing GM** (a fresh Claude given only "hi! let's play" and Familiar's tools) | Read the Table Rules, Start Here, the two right Source pages and Prep, then posted a faithful, spoiler-free recap with both of the book's choices. Familiar's per-tool call counters matched its report exactly. It also caught Kestrel's walk speed of 0 | Check PC sheets too (no species means speed 0) |
| **Cold preparing AI** (a fresh Claude given only the skill and "prep the next session") | Filed the session log, audited Prep 1 against the Source (caught a wrong max-damage line and a missed round-2 tactic), wrote Prep 2 with a clue check and danger check, and reported honestly. Listed 12 gaps in the skill | All 12 fixed: how far ahead to prep, actors for reachable fights, "file" vs "move", disconnect handling, line breaks after block tags, plain-text reads, spoilers in Trackers, memory fixes are the GM's call, which sheet sections to read, session numbering |
| **Walls and lighting** (Dragon's Lair) | 23 walls traced from the book's map. A torch in darkness, seen through the player's token, stopped at the walls. The GM's screenshot showed no darkness at all | Canvas tools act on the active scene only; build at a quiet moment and verify from a player's view |
| **Audio** | Playlist staged, not playing; file served (200); a bad path 404s | Familiar doesn't check audio paths, so you do |
| **Combat** (Deep Scion fight, played to it for real) | The runner read the combat sheet and Prep, switched scene, placed tokens **at the Prep's exact pixel positions**, started combat, rolled initiative, applied surprise from the sheet, and resolved Psychic Screech at the book's DC 13 | Works |
| | Multiattack resolved as **bite + bite**, not bite + claw + claw | Familiar parses the Multiattack text: write the fighting form first and use plain weapon names. Verified fixed: bite 1d4+4, claw 1d6+4, claw |
| | On the way: one branch skipped (entry 51's choice), a DC announced before the roll, heavy rules and tool chatter in public chat, items created as actors (Familiar's approval gate stopped the delete) | Runner Guide now says where gear goes and "never delete actors". The rest are model-behaviour limits (DeepSeek V4 Flash) |
| **Resume rule** | A GM's deliberate skip-ahead checkpoint was twice "corrected" back to older chat | Only chat *newer* than the Pickup overrides it; GM edits are deliberate |
| **Multi-player** | A second (non-solo) player asking for spoilers got a friendly refusal. No dragon name, though the notes hold it | Works |
| **Connection** | The GM tab's link dropped several times. Familiar reported `connected: true` while calls timed out | Don't trust diagnostics over failing calls; read back before retrying a write |

Still unproven after round 2: systems other than D&D 5e (only the author's earlier Vice & Violence setup suggests the journal and handoff parts carry over), a full combat to the end, and the corrected resume wording in a fresh table-chat session. Table chat keeps its own conversation memory, so mid-conversation prep edits only land at the next session start.
