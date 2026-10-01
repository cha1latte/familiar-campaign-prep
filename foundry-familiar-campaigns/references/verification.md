# Verification

Size the checks to the job. A fixed token needs a look at the token, not a campaign audit. A full install needs every gate below.

## Readiness gates

Each claim needs its own evidence. Never roll them up into "all working".

| Claim | Evidence required |
|---|---|
| Source imported | Inventory written; next-session sections compared against page images; skipped or sampled parts listed |
| Prep is faithful | Every consequential fact in the opening prep (times, numbers, DCs, NPC knowledge, rewards) traced to its Source page; adaptations listed |
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
| Fails an important check | Consequence from the source; the fact still exists; another route remains |
| "ok I attack" with an enemy out of reach | Moves first or says it can't reach; no free hit |
| "let's stop for tonight" | Runs closeout: Pickup, Log and Trackers updated and read back |
| New chat, "continue" | Resume recap matches the new Pickup exactly |

## Review after real play

After a session or two, look at the chat log and the records together:

- Did the runner read the source at each new section (tool log), and did the narration match it?
- Did choices matter? Were the source's alternatives honoured, and off-script ideas handled fairly?
- Did NPCs stay within their knowledge?
- Were rolls, HP, items and rewards each handled exactly once?
- Did closeout leave a Pickup that a fresh chat could run from?

Fix the **smallest layer that failed** (see the [drift audit](source-intake.md#drift-audit)). Don't grow the Table Rules every time something goes wrong. Untested behaviour (an event that never fired, a closeout that never happened) is *untested*, not *passed*.

## Reporting honestly

Use these words consistently:

- **Saved**: a write succeeded.
- **Read back**: you re-read it and it matches.
- **Rendered**: you looked at a screenshot.
- **Retrieved by the runner**: the playing AI's tool log shows the read.
- **Worked in play**: observed in an actual session or test turn.

Say which ones you have. "Prep saved; handoff not verified" is a perfectly good outcome when testing wasn't possible.
