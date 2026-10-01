# Chai's Familiar Campaign Prep Skill

**Version 2.0 (October 2026)** · for Foundry VTT 13–14 with [Familiar](https://familiarvtt.com) 2.x

Ever wanted to run a premade adventure with Familiar, only to have the AI set it up badly and then slowly drift away from the actual plot? This skill teaches your AI assistant to prepare the game properly. It puts the **original adventure text** into Foundry, builds the scenes and characters, and installs short instructions that make the playing AI read the right notes at the right time. That covers session one, and every session after it.

## What you get in Foundry

```text
📁 Frozen Offerings (GM)
   📕 Frozen Offerings: Start Here    ← the AI GM reads this every session
        Runner Guide · Pickup · Table Profile · Adaptations · Trackers · Session Log
   📗 Frozen Offerings: Source        ← the adventure's own text, section by section
   📘 Frozen Offerings: Prep 1        ← runnable notes for the next stretch of play
🗺️  Scenes from the adventure's maps (staged, not activated)
🧝 Actors: sidekicks, NPCs and monsters from the book's stat blocks
⚙️  Table Rules: a short block telling the AI GM where to look and what not to do
```

When you stop playing, the AI GM saves a "where we are" Pickup (with a short summary of the session) after re-reading the original text. Next time you ask your prep assistant to *"prep the next session"*, it files that summary into the Session Log and preps what's ahead, from the original text. Next time, "hi, let's play" is enough: it reads the notes, recaps and waits for you.

## Install (2 minutes)

1. Unzip this file.
2. Give the `foundry-familiar-campaigns` folder to your AI assistant and ask it to **install it as a skill**. Claude Code: `~/.claude/skills/`. Codex: `~/.codex/skills/`. Others: wherever your assistant loads skills or project instructions from. Keep the folder intact.
3. Start a new chat with your assistant so it picks the skill up.
4. Make sure Familiar is connected: Foundry open as GM, Familiar's MCP server attached to your assistant.

No assistant skill support? Put the folder somewhere your assistant can read, and say *"Follow foundry-familiar-campaigns/SKILL.md"*.

## Then just ask

| Say | What happens |
|---|---|
| *"Find me a free one-player D&D adventure and set it up."* | Shortlists legitimate options (no pirate sites), lets you pick, then installs it |
| *"Set up this adventure: ~/Downloads/my-adventure.pdf"* | Imports the text faithfully (two-column PDFs included), builds journals, maps, actors and Table Rules |
| *"Prep next session."* | Reads where you stopped, re-reads the original, writes the next prep |
| *"We're done for tonight, save everything."* | Session log, Pickup, trackers and campaign memories, all read back |
| *"The AI said the dragon had 3 heads. Why?"* | Traces that fact from the book to the reply and fixes the layer that broke |
| *"Fix the cave map's walls."* | Just the scene work |

**Playing as a player, with the AI as GM?** Say so ("I'm the player, no spoilers"). The prep assistant then keeps secrets out of its replies to you, and out of anything you can see in Foundry.

**Solo with Familiar's table chat** (you type `@familiar …` in Foundry chat)? The skill sets up the three things Familiar requires: your player account with your character *linked* to it, your token on an active opening scene, and you as Familiar's Solo Player. Without them Familiar politely refuses to play.

## How it keeps the story straight

- **The original text lives in Foundry** and wins over any summary. Deliberate changes (solo scaling, house rules) go in an *Adaptations* ledger, never silently into the notes.
- **The Table Rules stay short.** Familiar caps them at 3,000 characters, so the skill installs a ~1,100-character pointer and merges it with your existing rules instead of overwriting them.
- **It checks its own work.** It reads back every page, screenshots the scenes, and when allowed, tests a fresh "hi, let's play" with the real playing AI. It tells you plainly when something is *saved but not verified*.

## Tested for real

This version was run end to end, not just written. A real free solo adventure (*Frozen Offerings*, from *Dragon+* 34) was installed in a test world. Familiar's own AI then played it from the player's side across two sessions: opening, play, stop, resume, stop. The adventure text went in **byte-exact**, and the creatures match their **printed stat blocks to the number**. The AI GM read the notes (its tool receipts show it), narrated faithfully, stopped where the player had to decide, and saved its place. A second round then tested the hard parts. An outside AI (Claude) ran the game cold. A fresh AI prepped the next session using only this skill. The first fight ran on the book's own map with tokens where the prep put them. Walls, lighting and audio were checked from the player's view, and a second player tried and failed to get spoilers. Everything that went wrong along the way is fixed in the skill and written up in `references/examples/frozen-offerings-walkthrough.md`.

## Good to know

- **Privacy:** Familiar's *Auto Context Retrieval* (on by default) sends matching journal excerpts, including GM-only ones, to your chat provider with each message. That's how the built-in chat gets context. Turn it off in Familiar's settings if you don't want that.
- **Your adventure stays yours.** The skill contains no adventure text, maps or art. Whatever you import stays in your private world. Respect your adventure's licence and don't share imported copies.
- **Costs:** the skill itself is free and installs nothing. Generating maps or art uses your own image-provider credit, and the assistant will ask first. It prefers the adventure's own maps.
- **Systems:** works with any Foundry game system for journals, scenes and handoff. Familiar's automatic combat rules are D&D 5e (2024) only.
- **Limits:** instructions improve consistency; they can't force a model to behave. In testing, one chat model (Gemini 3.8 Flash) read the notes and then returned empty answers on every turn, while DeepSeek V4 Flash played well. If Familiar keeps saying "didn't have an answer for that", try another model. The skill's handoff test shows which it is.

## Troubleshooting

| Problem | Try |
|---|---|
| The AI GM doesn't seem to know the campaign | Ask your prep assistant to *"test the handoff"*; it checks the GM's tool receipts to see whether it actually reads the notes |
| Familiar says "no character is linked" or "no token on the active scene" | Ask your prep assistant to *"set me up as the solo player"* |
| My old Table Rules disappeared | Familiar replaces the whole text on each write. The skill merges and keeps a backup page; ask it to restore |
| Players see a black map | Ask to *"check player vision on <scene>"* |
| Notes contradict what happened at the table | Ask for a *"drift audit"*. Played events are kept, and the notes get fixed |

## What's inside

```text
foundry-familiar-campaigns/
  SKILL.md                         the workflow (start here)
  references/
    familiar-facts.md              verified Familiar tool contracts, limits, gotchas
    finding-adventures.md          legit sourcing; gamebook / duet / party-run-solo notes
    source-intake.md               faithful PDF import; adaptations ledger; drift audit
    journal-layout.md              the journal layout + page templates (Runner Guide, Pickup…)
    runner-setup.md                Table Rules block; safe install; handoff test
    session-prep.md                runnable prep: NPC knowledge cards, clues, encounters
    scene-production.md            maps, walls, light, tokens, actors, audio
    closeout-and-resume.md         end-of-session and pick-up procedures
    verification.md                readiness gates; behaviour checks; honest reporting
    dnd/session-craft.md           D&D 5e: edition checks, solo danger, gamebooks
    examples/frozen-offerings-walkthrough.md   a full real install, step by step
  agents/openai.yaml               display metadata for Codex
```

## Changes from v1

v1 laid down good principles. v2 makes them usable:

- **Grounded in what Familiar actually does:** a verified tool map, the 3,000-character Table Rules cap, GM-only defaults, auto-context behaviour.
- **One clear 8-step workflow**, with a "pick the job" table.
- **Concrete journal layout with templates.** The Runner Guide carries the closeout and resume procedure, so the playing AI has it even without this skill.
- **New:** finding adventures, two-column PDF extraction, gamebook support, solo table-chat setup, actor read-back checks, a handoff test with tool receipts, a one-write closeout the AI actually completes, and a full worked example tested live in Foundry.
- About the same size, organised to be read by people as well as AIs.

Made by Chai. Share freely; feedback welcome.
