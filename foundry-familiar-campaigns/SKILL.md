---
name: foundry-familiar-campaigns
description: Use when setting up, preparing, continuing or auditing a tabletop campaign in Foundry VTT through Familiar. Covers finding or importing a premade adventure (PDF, module, notes), building its journals, scenes and actors, writing the playing AI's Table Rules, end-of-session summaries, next-session prep, resuming play, and repairing drift from the original adventure. Not for running a live combat turn or a quick rules question.
---

# Foundry + Familiar Campaigns

Turn adventure material into a Foundry world that an AI game master (Familiar's built-in chat, or an outside assistant using Familiar's tools) can run **faithfully, session after session**. You are the *preparing* assistant. The *playing* assistant usually won't have this skill, so everything it needs must end up inside Foundry, where it can actually read it.

Four rules shape everything below:

1. **The original is the authority.** Keep the adventure's own text in Foundry, section by section. Summaries help; they never replace it. Adaptations go in a ledger, not silently into the prep.
2. **Put things where they're consumed.** Familiar's Table Rules (hard cap 3,000 characters, sent with every prompt) carry a short pointer and a few must-follow rules. The detail lives in GM-only journals. Local files, this skill and your own memory don't reach the playing AI.
3. **Preparation is not play.** Don't start scenes for players, move the party, advance time, roll live encounters, spend resources or reveal anything while preparing. Stage things inactive.
4. **Prove it before you claim it.** "Saved", "read back", "the playing AI retrieved it" and "worked in play" are four different claims. Report each one separately and honestly.

## 1. Orient (always, and keep it short)

1. `get-world-info`. Note the system and version, existing Table Rules (`tableInstructions`), campaign memory and who's connected. If the system isn't `dnd5e`, Familiar's combat automation doesn't apply: use the system's own rolls and say so.
2. `list-tool-bundles` and enable what the job needs: `journals`, `scenes`, `knowledge`, `character-edit`, `canvas-environment` and `scene-generator`.
3. Look before you build: `list-journals`, `list-scenes`, `list-characters`, `list-folders`. Reuse what exists. If a previous prep run left a "Start Here" journal, read it first. You may be continuing work, not starting it.
4. Work out the audience. Who is the GM, who plays, and is the person talking to you a **player who must not see spoilers**, even on a GM login? If they're a player, keep secrets out of your replies to them, out of document names and out of chat. Only ask when the answer isn't discoverable and it changes what you'd build.

[Familiar facts](references/familiar-facts.md) lists the verified tool contracts, limits and gotchas. Read it before your first write in a session.

## 2. Pick the job

| The user wants… | Do this | Main reference |
|---|---|---|
| "Find me an adventure" (nothing supplied) | Shortlist legitimate options, let them pick, then *Install* | [Finding adventures](references/finding-adventures.md) |
| A premade adventure set up for the first time | **Install**: steps 3–8 | [Source intake](references/source-intake.md), [Journal layout](references/journal-layout.md) |
| The next session prepared (campaign already installed) | File the runner's *Last session* into the Session Log and Trackers, then steps 4–8 for what the next session can reach, including actors and scenes for any newly reachable fight | [Session prep](references/session-prep.md), [Closeout audit](references/closeout-and-resume.md#auditing-a-closeout-preparing-assistant) |
| To end tonight's session / "save where we are" | **Closeout** | [Closeout and resume](references/closeout-and-resume.md) |
| To pick up where they left off | **Resume** check | [Closeout and resume](references/closeout-and-resume.md) |
| A map, scene, token or actor fixed or made | Scene work only; skip the rest | [Scene production](references/scene-production.md) |
| "Why did the AI get X wrong?" / drift audit | Trace source → prep → rules → reply | [Source intake § Drift audit](references/source-intake.md#drift-audit) |

Match the scope. A scene request gets a scene, not a campaign overhaul. A D&D 5e adventure also uses [D&D session craft](references/dnd/session-craft.md).

## 3. Get the source in, faithfully

Inventory what you have: the PDF/module/notes, its version, page count, maps, handouts, stat blocks and pregens. Extract the text with layout awareness (two-column PDFs scramble under naive extraction), then **check the extraction against the page images** for the sections you'll use first. Store it in a GM-only **Source** journal, one page per section or entry group, each page headed with its printed page reference. Push long text by script where you can rather than retyping it, then read every page back and diff it against the extraction. → [Source intake](references/source-intake.md)

Respect the licence. Personal-use material goes into the user's own private world only. Never redistribute it, and never put it in this skill, memory or a public post.

## 4. Build the campaign journals

Create the standard layout from [Journal layout](references/journal-layout.md):

- **`<Title>: Start Here`** (small; the playing AI reads it in full every session): *Runner Guide*, *Pickup*, *Table Profile*, *Adaptations*, *Trackers*, *Session Log*.
- **`<Title>: Source`**: the original, section by section.
- **`<Title>: Prep <segment>`**: runnable notes for the next stretch of play, with NPC knowledge cards and encounter checks, each linked back to its Source page.
- **`<Title>: Handouts`** (optional): player-safe material the GM shows by hand.

Everything Familiar creates starts GM-only, and Familiar can't change ownership. Tell the GM which documents (if any) to share manually.

## 5. Make it playable in Foundry

**Solo game with Familiar's table chat?** It won't run until the player's account has a *linked* character, that character has a token on the **active** scene, and the player is set as the Solo Player. See [Familiar facts § Solo play](references/familiar-facts.md#solo-play-through-table-chat-what-it-needs-all-observed-live). Making the opening scene active, with the PC's token on it, is the one activation that belongs to prep. Do it last, and only when nobody is mid-session.

Build what the table actually uses. If they play on maps: scenes for the opening and its likely next locations, walls and lights where they matter, actors for the NPCs and creatures in play, the sidekick or pregen as a real character sheet, and player ownership set. If they play theatre-of-the-mind, skip the tactical work and say so. Prefer the adventure's own maps and art over generated ones. Read every actor's **computed** numbers back against the printed stat block (AC, HP, saves, skills, to-hit, damage). dnd5e silently drops or recomputes several fields. Keep future scenes inactive and hidden from navigation. → [Scene production](references/scene-production.md)

## 6. Write the playing AI's instructions

Merge the compact block from [Runner setup](references/runner-setup.md) into the existing Table Rules. Read them first, keep the table's own rules, stay under the cap, write once and read back. The block names the Start Here journal (name **and** ID) and tells the runner to read the Runner Guide and Pickup at session start, use the Source pages for the section in play, and run Closeout when the player stops.

## 7. Verify

Use the readiness checks in [Verification](references/verification.md). At minimum:

- Every document you wrote reads back intact, with no truncation and nothing missing.
- The opening section passes a "could another GM run this without inventing its answers?" review.
- Scenes render. A screenshot shows the map, tokens and lighting. Players can see what they should, and nothing they shouldn't.
- **Handoff.** If you're allowed to test the playing AI, a plain "hi, let's play" in a fresh chat should make it read Start Here and the opening Source page, recap without spoilers, and wait. Inspect the actual tool calls (table chat leaves a GM-only receipt listing them) and the reply. If you can't test it, the honest status is *"Prep saved; handoff not verified."*

## 8. Hand over

End with a short receipt for the person in front of you. Keep it spoiler-safe if they're a player:

```text
Ready:      what can be played now, and the first thing to say to start
Built:      journals / scenes / actors / Table Rules changes (names, not secrets)
Verified:   saved+read back / rendered / playing-AI retrieval / tested in play
Not done:   gaps, unverified parts, anything that needs the GM's hands (sharing, secrets, keys)
Next:       the one next action
```

## References

| File | Read when |
|---|---|
| [familiar-facts.md](references/familiar-facts.md) | Before your first Foundry write; whenever a tool surprises you |
| [finding-adventures.md](references/finding-adventures.md) | No adventure was supplied |
| [source-intake.md](references/source-intake.md) | Importing material; auditing drift |
| [journal-layout.md](references/journal-layout.md) | Creating or repairing campaign journals (has page templates) |
| [runner-setup.md](references/runner-setup.md) | Writing Table Rules; testing the handoff |
| [session-prep.md](references/session-prep.md) | Preparing a section or session; NPCs, mysteries, encounters, clocks |
| [scene-production.md](references/scene-production.md) | Maps, walls, lights, tokens, actors, audio |
| [closeout-and-resume.md](references/closeout-and-resume.md) | End of session, next-session prep, resuming |
| [verification.md](references/verification.md) | Before saying "ready"; reviewing after play |
| [dnd/session-craft.md](references/dnd/session-craft.md) | D&D 5e content: edition checks, encounter safety, runnable-packet review |
| [examples/frozen-offerings-walkthrough.md](references/examples/frozen-offerings-walkthrough.md) | You want to see the whole flow done once |
