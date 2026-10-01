# Closeout and resume

The playing AI does closeout and resume, following the Runner Guide page you installed. You do them yourself when the user asks *you* to wrap up or pick up a session, or when you're auditing whether the runner did them properly.

## Closeout: end of a session

Trigger: the player says they're stopping ("let's stop here", "save the game", "that's it for tonight").

1. **Land the beat.** Finish the current exchange without starting anything new. Don't move the party or advance time just to make a tidier stopping point.
2. **Gather the facts** from what actually happened. Use the Foundry chat log (`get-recent-messages`) and the sheets, not memory alone: the entries/sections covered, choices, rolls that mattered, HP and resources, items gained or spent, codewords, promises.
3. **Read ahead.** Read the Source page(s) for where the party is heading next. Next session's prep comes from the original, not from tonight's narration.
4. **Write the records.** Keep the runner's part to *one* page write. In the live test, the playing AI reliably made one write and dropped a second and third.
   - **Pickup**: overwrite it with the new checkpoint, including a **Last session** block (3–6 lines, facts only). Write HTML with one `<p>` per heading: plain text with newlines shows up as one unreadable blob in Foundry. Its *Read next* line names the Source and Prep pages for the next stretch.
   - **Campaign memory**: 3–8 durable facts with `save-campaign-memories` (one fact each, ≥20 characters, a sensible category). Durable means still true next month: relationships, oaths, items of note, places discovered. Not "they're in the cave", which belongs in the Pickup.
   - **Preparing assistant, at the next prep**: copy the Last session block into the **Session Log** (newest first). Then replace it in the Pickup with "(filed to Session Log, <date>)", so it isn't filed twice. Update **Trackers** from the chat log, and write the next **Prep** page if it's missing. When *you* are the one closing out, do all of it now.
   - **Session numbers**: number each sitting in order (Session 1, Session 2, …), even short ones. The Pickup's *Updated* line names the session that just ended.
5. **Read back** the Pickup (`get-journal-page`). In the live test this step was the one most often skipped, so it's worth its own line in the Runner Guide. If a write failed, say exactly what's saved and what isn't. Never claim a full save.
6. **Sign off** in the table's voice: one spoiler-free line about next time.

Keep these separate in everything you write: **what happened** (log), **where we are** (pickup), **what might happen** (prep, GM-only), and **what we changed from the book** (adaptations). If play contradicted the source, log what happened, add a line to the Pickup's *Warnings*, and let the GM decide whether it becomes an adaptation.

## Resume: start of a session

Trigger: any ordinary opener ("hi", "let's play", "continue"). No special phrase needed.

1. Read Start Here in full, then the Source and Prep pages the Pickup names.
2. Compare with live Foundry: the latest chat messages, active scene, token positions, HP. Only chat posted **after the Pickup was last updated** (a closeout that didn't happen) overrides it. Older chat never overrides a newer Pickup. A GM's edit to the Pickup (a skip-ahead, a retcon) is deliberate: follow it. In testing, a runner given the looser "chat wins" wording twice tried to revert a GM's deliberate checkpoint to match older chat. A message after closeout that changes nothing (a GM test post, a repeated recap) is noted in the Pickup's *Warnings*, so it isn't mistaken for play. Never replay it. Bring the Pickup up to date first and tell the GM what you reconciled.
3. Recap what the characters know, in two to four lines, spoiler-free. Restate the pending decision.
4. **Stop and wait.** Resuming doesn't advance time, start a scene, roll anything, or act for the player.

If the Start Here journal can't be read (renamed, deleted, permission), don't improvise a recap from memory or excerpts. Say what's missing and ask the GM. If only a minor page is missing, carry on with what you have and name the gap.

## Auditing a closeout (preparing assistant)

After a session, check the runner's work against the chat log:

| Check | Problem signs |
|---|---|
| Pickup matches the last chat beat | Position, HP or pending choice is stale or invented |
| Session Log is facts only | Plans, guesses or future events written as history |
| Trackers updated once | Duplicate rewards, missing codewords, payment counted twice |
| Next prep came from the source | Prep that only paraphrases tonight's narration |
| Memories are durable and true | Temporary positions, secrets saved where players could see them. **Report** bad ones to the GM, with a suggested fix. Correcting or deleting campaign memories is the GM's call |

Repair the records from the evidence. Never re-run or "replay" an event to make the notes line up.
