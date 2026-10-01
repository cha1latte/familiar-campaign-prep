# Familiar facts for campaign prep

Checked against **Familiar 2.25.0 on Foundry 14**, October 2026. Tool contracts change between versions. If a call is refused or a result doesn't match this page, believe the live tool description and the actual result. Use `get-server-diagnostics` to see the running version.

## How the pieces fit

```text
 Preparing assistant (you)  ──MCP tools──►  Familiar server  ◄──websocket──  Foundry tab (GM)
 Outside playing assistant  ──MCP tools──►        │                              │
 Familiar built-in chat (owl) ────────────────────┴── runs inside the GM's browser
```

- The Familiar tools act through a **GM's open Foundry tab**. If no GM has the world open, nothing works. Check `get-server-diagnostics` → `foundry.connected` / `authenticated`.
- The default build only accepts a Foundry served on `localhost:30000`. Moving a world to another port breaks the bridge.
- **There are three possible playing assistants.** They all receive the Table Rules. The first two run on the GM's configured chat provider and get automatic lore lookup (below):
  1. **Built-in chat**: the owl button in the GM's Foundry tab.
  2. **Table chat**: players type `@familiar …` in ordinary Foundry chat (and `@NPCName …` to talk to an NPC). It's on by default (`tableChatEnabled`), and a per-session turn budget (`tableChatTurnBudget`) switches it off when spent. Naming a **Solo Player** (`tableChatSoloUser`, a player account's user ID from `list-users`) runs that player's table turns with the GM's own rules. That's the natural setup for a one-player campaign with an AI GM.
  3. **An outside assistant** connected over MCP (Claude, Codex…), which plays from its own chat window.

## Solo play through table chat: what it needs (all observed live)

Table chat's Solo Player refuses to play until each of these is true:

1. **The player's user has a linked character** (Foundry *User → Character*), not just ownership of an actor. Familiar has no tool that links one to an *existing* user. Either create the player's account with `create-user { name, role: 1, character: <actorId> }`, or have the GM use *Familiar settings → Chat → Solo check*. Then name that user in `tableChatSoloUser`.
2. **That character has a token on the active scene.** Otherwise the solo check replies "Your character has no token on the active scene". Even a theatre-of-the-mind opening needs an active scene, which can be a simple backdrop, with the PC's token on it.
3. **The chat model can answer after a tool call.** In testing, Gemini 3.8 Flash (via NanoGPT) read the journal correctly and then returned empty text on every turn ("Familiar didn't have an answer for that."). DeepSeek V4 Flash answered properly. If you see that message repeatedly, it's the model, not the prep. The model choice is the GM's, so tell them; don't switch it yourself.

Each table-chat turn leaves a **GM-only receipt** in Foundry chat: "Table chat — <user> · solo authority · N model calls", followed by the tools called (✓ get-journal…). That's your evidence for the handoff check. Read it with `get-recent-messages`.

## What reaches the playing AI

| Channel | What it carries | Limits |
|---|---|---|
| **Table Rules** (`customInstructions`, world setting) | Appended to every built-in chat prompt; returned to MCP clients as `tableInstructions` in `get-world-info` | **3,000 characters.** A write replaces the whole text. `update-familiar-setting` accepts up to 4,000, but the setting keeps 3,000. Changes apply from the next turn. |
| **Auto context retrieval** (`autoContextRetrieval`, client setting, on by default) | Before each built-in chat reply, a keyword (BM25) search of journals, actors and items. The top matching *excerpts* are attached. | Excerpts, not whole pages. GM-only journals can match too, so their text goes to the chat provider. It doesn't prove the runner read the current Pickup. |
| **Campaign memory** (knowledge bundle) | Up to 300 short facts; a digest of the top facts rides in `get-world-info` | Good for durable facts ("Thalgar owes the party a favour"). Too small and too unordered for a checkpoint or source text. |
| **Journals** (journals bundle) | Whatever the runner explicitly reads with `get-journal` / `get-journal-page` | Only what it actually reads. Tell it exactly what to read. |
| **Actor biography** (`system.details.biography.value`) | Never read when Familiar voices an NPC in table chat | Safe place for an NPC's private notes. Public persona text belongs in other fields. |

## Journal tools: contract notes

- `create-journal { name, folderId?, pages:[{name, content}] }` returns the journal ID and page IDs. Pages take HTML. Each page holds up to 50,000 characters, and a journal can take up to 100 pages in one call.
- New journals are **GM-only** (default ownership None). Familiar has **no tool to change journal ownership or move a journal to another folder**. Pick the folder at creation. Sharing is the GM's manual job (ownership, or *Show Players*).
- `get-journal { identifier }` returns **every page as plain text**. That's fine for a small Start Here journal and floods the context for a big Source journal. Read Source pages one at a time with `get-journal-page { identifier, pageId }`. A journal ID and a page ID are not interchangeable.
- `update-journal-page` **replaces** the whole page. Read it, change it, write it back. The journal-level `update-journal` only renames.
- `list-journals` gives names, IDs, folders and page names/IDs. `search-journals` gives snippets, capped at 25 results.
- Lookups accept a name or ID. Names with plain ASCII punctuation (`Frozen Offerings: Start Here`) are the easiest to match. Always record IDs as well.

## Settings

- `get-familiar-settings { area: "system-rules" }` reads the Table Rules. `update-familiar-setting { key: "customInstructions", value }` writes them. Some settings are `gm-confirm`: a GM may have to click *Yes* in the Foundry tab, and the result reports `confirmed`.
- Secrets, API keys and licence keys can't be read or written through tools. Ask the GM.
- `autoContextRetrieval` and other client-scope settings belong to *that GM browser*. A second GM login keeps its own values.

## Scenes, canvas, actors

- `create-scene` makes a scene from an existing image path (local OS paths don't work; use a Foundry-served path). `generate-scene` creates an AI background **with no walls or tokens**, spends the GM's image-provider credit, and returns `backgroundMapping` for converting image pixels into canvas coordinates (padding included).
- `create-scene` with `activate:true` **pulls every player to the scene**. Never do that during prep. `create-scene` has no folder argument, and `update-scene` strips `folder`, so scenes stay at the sidebar root.
- `create-tokens`, `create-walls`, `create-light` and their list/delete tools act **on the active scene only**. See [Scene production § Staging](scene-production.md#staging-not-starting) for how to pre-plan placement instead.
- `create-user` exists (roles 1–4, passwordless). A separate Assistant-GM account is handy for scripted checks without touching the GM's own session.
- Walls, lights, darkness and weather are in `canvas-environment`. Tokens are in `scenes`/`canvas`. `get-screenshot` captures the **GM's current view**, annotated with token badges. It's proof of layout, not proof of what a player sees.
- `create-character` accepts the system's own data. On dnd5e, `create-character-from-compendium` is the fast route to SRD stat blocks. A `set-table-npc` pin makes Familiar answer *as* that NPC when a player writes `@Name` in Foundry chat.
- **Familiar saves chat sessions as journals** (`sessionPersistence`, on by default) in a "Familiar Sessions" folder. They're GM-only, but they're journals: auto-context lookups can match them, and they pile up. Don't mistake them for campaign records.
- Familiar's combat resolvers (`resolve-attack` and friends) enforce **D&D 5e 2024** rules only.

## Chat and records

- `send-chat-message` posts to Foundry chat. Enricher syntax such as `@UUID[...]` is neutralised, so write plain prose. Whispers go through `whisperTo`.
- Generating a sound effect posts a public "AI Sound" card describing it. Don't generate spoiler-laden sounds while players are watching.
- `get-recent-messages` reads the chat log (whispers you can see, too). Use it to check what the playing AI actually said.

## Gotchas that have bitten real tables

- **Rules that never arrived.** A long play guide pasted into Table Rules gets cut at 3,000 characters, and anything you appended after it vanishes. Keep the block short and put it near the **top**.
- **Overwritten table rules.** Writing Table Rules through the tool replaces whatever the GM or another module wrote. Read first, then merge.
- **"It said it read the notes."** A runner that announces it loaded the prep, without a `get-journal` call in its tool log, has only seen excerpts at best.
- **Players see black maps.** A token without vision on a dark scene sees nothing. Turn on global illumination for daylight scenes. Don't fix visibility by switching off token vision or resetting fog; that exposes what should be hidden.
- **Activated a scene early.** Activation moves everyone. Prep stays inactive.
- **Familiar went quiet.** Tools fail with `FOUNDRY_DISCONNECTED` when the GM tab's link drops: a tab the browser put to sleep, or a tab that was closed. Twice in testing the link came back by itself within a minute (auto-connect). Once it flapped for about six minutes, failing as `QUERY_TIMEOUT` while `get-server-diagnostics` still said `connected: true`. So:
  - Don't trust diagnostics over a failing call.
  - Wait, then retry. **After a failed write, read the page back before retrying**, because a timed-out write may or may not have landed.
  - If it stays down, carry on with work that doesn't need Foundry, report exactly which writes are pending, and ask the GM to reload their tab when they're back. Keep the GM's Foundry tab in its own window, or use the Foundry desktop app.
