# Verification

Size the checks to the job. A fixed token needs a look at the token, not a campaign audit. A full install needs every gate below.

## Readiness gates

Each claim needs its own evidence. Never roll them up into "all working".

| Claim | Evidence required |
|---|---|
| Source imported | Inventory written; next-session sections compared against page images; skipped or sampled parts listed |
| Prep is faithful | Every consequential fact in the opening prep (times, numbers, DCs, NPC knowledge, rewards) traced to its Source page; adaptations listed |
| Section cards match the book | Every number, name and exit on each card found in its Source page (by script where possible); each card under about 1,500 characters; card names spoiler-safe; the Pickup names the current card |
| Prep is runnable | The five-attempt walk-through in [Session prep](session-prep.md#review-it-by-trying-to-run-it) passes with no invented central answers |
| Journals saved | Every page read back: right journal, full length, no truncation; IDs recorded in the Runner Guide |
| Private stays private | Journals GM-only (default); no secrets in names, chat, memories or handouts |
| Table Rules installed | `get-world-info` → `tableInstructions` shows the merged block intact, ≤ 3,000 chars, the table's own rules preserved |
| Scenes configured | Background loads; grid matches the art; walls/lights as planned; scene inactive |
| Scenes look right | `get-screenshot` reviewed: art, tokens, light stopping at walls |
| Players can see | Checked from a player's view, or flagged as unchecked |
| Actors correct | Sheet read back against the source stat block; ownership set |
| Solo table chat ready | Player account has a linked character; that character has a token on the active scene; `tableChatSoloUser` set (see [Familiar facts](familiar-facts.md#solo-play-through-table-chat-what-it-needs-all-observed-live)) |
| Handoff | Fresh-chat test passed all four checks in [Runner setup](runner-setup.md#proving-the-handoff), with tool receipts as evidence, or reported as not verified |
| Closeout works | A test or real closeout produced a correct Pickup, Log and Trackers (read back) |

## Quick behaviour checks

If you can test the playing AI on a copy of the world or at a moment the GM chooses, these are cheap and revealing. Use invented situations where possible, so real secrets don't end up in test prompts.

| Prompt to the runner | Expected |
|---|---|
| "hi, let's play" (fresh chat) | Reads Start Here and the Source page first; recaps; waits |
| "what's in the next room?" (before entering) | Describes only what's perceivable; no spoilers |
| A sensible plan the source didn't foresee | A fair ruling, recorded; no railroading back to the script |
| Mid-section, a question the card answers ("what's the DC to climb the wall?", "does the miller know about the cellar?") | Tool log shows the card read *before* the reply; the answer matches the card; no DC revealed if the hint level forbids it |
| The GM approves a change ("ok, let's say the guard is asleep") | One write to *Table Changes* with the section tag and who approved; Source and the card unchanged; play carries on in-world |
| The player alone asks for a change at a table where only the GM approves | No write; the runner asks the GM |
| Fails an important check | Consequence from the source; the fact still exists; another route remains |
| "ok I attack" with an enemy out of reach | Moves first or says it can't reach; no free hit |
| "let's stop for tonight" | Runs closeout: Pickup, Log and Trackers updated and read back |
| New chat, "continue" | Resume recap matches the new Pickup exactly |

## Review after real play

After a session or two, look at the chat log and the records together:

- Did the runner read the source at each new section (tool log), and did the narration match it?
- Did choices matter? Were the source's alternatives honoured, and off-script ideas handled fairly?
- Did NPCs stay within their knowledge?
- Did the runner check the card before rulings, or answer first and look it up after being challenged?
- Is every change the table agreed to on *Table Changes*, and nothing it didn't?
- Were rolls, HP, items and rewards each handled exactly once?
- Did closeout leave a Pickup that a fresh chat could run from?

Fix the **smallest layer that failed** (see the [drift audit](source-intake.md#drift-audit)). Don't grow the Table Rules every time something goes wrong. Untested behaviour (an event that never fired, a closeout that never happened) is *untested*, not *passed*.

## The three gates

A campaign can pass every readiness check and still not be fun to play. Three questions, each scored **out of 10 from the player's chair**, each needing **8.5 or better**:

1. **Is it fun?** A hook in the first five minutes (a person in trouble, danger, a mystery); the player always knows what they want and a way to chase it; the player drives every scene (nobody demonstrates while the hero watches); a light load (one new invented word or system a scene, admin under five minutes and under pressure); payoffs, twists and session-end hooks; the hero's backstory, companions and class matter; variety, nothing over about 20 minutes of one kind of scene. **Capped at 6** if the first 15 minutes explain the world's systems, if a required sum or form comes before the first exciting thing, or if the player says they're lost or bored because of the design.
2. **Is it D&D?** (or the table's game) Exploration, combat that matters and social play every session; fights budgeted for the **actual party** (a steel defender, an animal companion or a sidekick counts) with one per session that can drop the hero; every bypass costs something; scenes for the hero's class and tools; loot, not ledger lines; fantasy, not an office in costume; rules that need no improvising. **Capped at 6** if a session spends more than about 40% of its time on non-adventuring, if session 1 has no fight or real hazard in its first half, or if fights were never re-budgeted for the party.
3. **Does it really teach?** (Teach mode) The player does the reasoning (a companion can do the sums); each idea is shown working with nothing at stake before it's tested; misconceptions are *felt*, not corrected in words; each idea returns three times; the world obeys the real mechanism and the decoder bridges to real words; it never feels like school. **Capped at 6** if an objective is never assessed in play, if a beginner's real-world quiz after the decoder is under 70%, or if the player said it felt like school.

**How:** score a transcript, not the book (book-only caps at 7). Use the real session's chat log, or a simulated playthrough: a GM agent with the book and a player agent with only the handouts, the character sheet and a profile of the real player, with real random dice on both sides, plus an out-of-character debrief. Give it to three cold judges, one per gate, who didn't write it; every criterion needs a quoted line or a page as evidence. Fix the lowest criterion at its root (the design, not the GM's wording), re-run what changed, re-score with fresh judges, and repeat until all three pass. **The real player overrules the judges.**

For a **premade adventure**, the book isn't yours to rewrite: put the fixes in prep (section cards, re-budgeted fights for the real party, a hook tied to the hero), in *Table Changes* the GM approves, or in a recommendation to the GM. For an **original or Campaign Writer package**, fix the design itself. The full rubric, the simulated-playthrough method and the fixes that usually work are in Campaign Writer's [three-gates.md](https://github.com/cha1latte/campaign-writer/blob/main/campaign-writer/references/three-gates.md).

## Reporting honestly

Use these words consistently:

- **Saved**: a write succeeded.
- **Read back**: you re-read it and it matches.
- **Rendered**: you looked at a screenshot.
- **Retrieved by the runner**: the playing AI's tool log shows the read.
- **Worked in play**: observed in an actual session or test turn.

Say which ones you have. "Prep saved; handoff not verified" is a perfectly good outcome when testing wasn't possible.
