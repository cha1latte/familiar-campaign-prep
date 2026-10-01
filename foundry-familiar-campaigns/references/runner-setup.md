# Runner setup and handoff

The playing AI gets two things automatically: the Table Rules, on every turn, and whatever journal excerpts auto-retrieval happens to match. Everything else it must be **told to read**. The Table Rules block below is the pointer. The Runner Guide page (see [Journal layout](journal-layout.md#0-runner-guide)) is the procedure.

## The Table Rules block

Fill in the angle-bracket fields with **real** names and IDs, after you've created the journals. The block is about 1,250 characters, leaving room for the table's own rules inside the 3,000 cap.

```text
CAMPAIGN: <Title> (<system>). Campaign notes are in the Foundry journal named "<Title>: Start Here" (look it up by that exact name; id <journalId> is a backup).
- Session start, a new chat, after a break, or "let's continue": read that journal in full (get-journal) BEFORE replying, then the Source/Prep pages its Pickup names (get-journal-page). Recap and stop at the pending decision. A greeting is not an action.
- New section or entry: read its Source page first. The original text wins over summaries; only the Adaptations page may change it.
- Don't invent clues, answers, rewards, NPC knowledge or difficulty. If it isn't written, look it up or ask the GM.
- No spoilers: never reveal unvisited content, DCs, stats or upcoming events. Never write the player character's words or decisions, even when the book scripts them.
- Post only in-world narration, NPC dialogue and direct answers. Never narrate your own tool use.
- When the player stops for the session: run CLOSEOUT from the Runner Guide (one Pickup write including "Last session", then memories, then read back) before saying goodbye.
```

Every line here earned its place in the live test:

- *Look it up by name.* The playing AI mistyped a 16-character ID (it dropped a `0`) and had to search to recover. Names are sturdier, so give the ID as a backup.
- *Never write the player character's words.* Gamebooks script "you say…" lines, and the runner copied two of them until this line was added.
- *Never narrate your own tool use.* Without it, "Now let me read the Backstory…" went into public chat.
- *One Pickup write.* Asked for three page writes, the runner did one.

Optional single lines, added only if the table wants them:

- `Hints: none. Explain odds only when asked.`
- `Combat: use Familiar's combat tools; the player rolls for <PC>.`

## Installing it safely

1. Read the current rules: `get-familiar-settings { key: "customInstructions" }`. If it isn't empty, save a copy as a page named `Table Rules backup <date>` in the Prep journal (GM-only, and outside Start Here so the runner doesn't re-read it every session), so it can be restored.
2. **Merge, don't replace.** Keep the table's own setting, tone, safety and house rules. Remove only an older block for the *same* campaign. Put the campaign block **first**, so that if anything gets cut it's the tail.
3. Count characters. The total must stay **≤ 3,000**. If it doesn't fit, shorten your block's optional lines first, then ask the GM which of their own lines can move into the Table Profile page.
4. Write it with `update-familiar-setting { key: "customInstructions", value }`. It's a `gm-confirm` setting, so a GM may need to click *Yes* in the Foundry tab, and the result says `confirmed`.
5. Read it back with `get-world-info`: `tableInstructions` must match what you wrote, end to end.

If several campaigns share one world, give each its own short block, name the active one first, and point each at its own Start Here journal. Better still, give each campaign its own world: Familiar also feeds the AI GM the world's campaign memories and table-chat history, and both point at whichever campaign was played last. When you switch a world to another campaign (tested live):

1. Start the Table Rules with an explicit line: `ACTIVE CAMPAIGN in this world: <Title>. The solo player is "<user>", playing <PC>. <Other campaign> is PAUSED: never read its journals, never mention it, ignore its campaign memories and old chat.`
2. Switch Table Chat off and on, to clear table chat's own turn history ([Familiar facts](familiar-facts.md#table-chat-keeps-its-own-history)). The GM panel's *New chat* doesn't do it.
3. Run the handoff test below. Before these two steps, the AI GM answered as the old campaign; after them, it read the right Start Here and resumed from its Pickup.

## Proving the handoff

"Saved" is not "the playing AI uses it". Test with the **actual** playing assistant, in a **fresh** chat, using an ordinary opener. No IDs, no special prompt:

> hi! let's play

Then inspect:

| Check | Pass looks like |
|---|---|
| Retrieval | Its tool log shows `get-journal` on Start Here, then `get-journal-page` on the Pickup's Source page, *before* the reply |
| Accuracy | The recap matches Pickup and Source (names, place, time of day, pending choice) |
| Restraint | No spoilers, nothing decided for the player, no time advanced, no scene activated, no rolls |
| Record | Chat log (`get-recent-messages`) shows exactly one reply where the table expects it |

Where the evidence lives:

- **Table chat** (players typing `@familiar …`): every turn leaves a GM-only receipt card in Foundry chat ("Table chat — <user> · N model calls · ✓ get-journal…"). Read it with `get-recent-messages`. To test as the player without disturbing the GM, log in as the player in a separate browser (or have the GM do it) and type the opener there.
- **Built-in chat** (owl): open the panel and expand the tool calls under the reply.
- **An outside assistant**: read its own tool transcript.

A test on a fresh setup took three tries in the live run. Each failure had a clear message, and each fix belongs in prep: no linked character, then no token on the active scene, then a model that returned empty answers. Fix the setup and re-test. Don't weaken the Table Rules to make a broken setup "pass".

Report the result in exactly these terms:

- **Handoff verified**: all four checks passed, plus a list of what was observed.
- **Prep saved; handoff not verified**: you couldn't run the test, or it was blocked. Say which, and what the GM should try at the next session start.
- **Handoff failed**: name the failing check and the layer it points to (pointer wording, journal name, permissions, model ignoring rules). Fix the smallest thing and test again in a new chat.

Rules:

- Don't run test turns in a game that's live with players present. Use a moment the GM picks, or a copy of the world.
- A test greeting can create a chat message. If the opener produced public narration, either leave it (it's a legitimate recap) or tell the GM. Don't delete the table's history.
- The runner saying "I've loaded the notes" proves nothing. Look at the tool calls.
- Re-test after renaming journals, changing the Table Rules, or switching the playing AI's model or provider.

## When the runner keeps ignoring the notes

Fix it in this order, testing after each step:

1. The pointer: is the journal name exact and the ID current? Does `get-journal` with that name work for you?
2. The wording: put the read instruction first and keep it imperative ("BEFORE replying").
3. Size: is Start Here so big that the runner skims it? Split old log entries out.
4. Model: some small or cheap models don't call tools reliably. Tell the GM plainly. It's their provider choice, so don't switch providers or models yourself.
