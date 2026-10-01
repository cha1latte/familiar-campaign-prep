# Scenes, actors and audio

Build what the table plays with. Check the Table Profile: a maps-and-tokens table needs real scenes; a theatre-of-the-mind table may want art only, or nothing.

## Decide each scene's job

| Job | Needs |
|---|---|
| Tactical (fights, chases, traps) | Top-down map, grid matched to the art, walls/doors, lights, tokens |
| Exploration (dungeon, building) | Top-down map, walls for line of sight, lights, entry point |
| Social / backdrop | One good image; grid and walls optional |
| Overland / travel | Region map; no walls; scale only if travel distance matters |

A pretty perspective illustration isn't a playable map. If the table plays on maps, a talky tavern scene still needs a usable floor plan.

## Find before you make

1. `list-scenes` and `search-scenes`. Repair an existing scene; don't duplicate it.
2. The adventure's own maps: extract them from the PDF at full resolution (for example `pdfimages -png` or render the page). Upload to a **Foundry-served** path and use `create-scene { background }`. A local `C:\…` path won't load in the browser.
3. Compendium/module scenes if the adventure ships as a Foundry module.
4. Only then generate: `generate-scene` (spends the GM's image credit). Prompt it with the source's layout facts, read the returned `backgroundMapping`, and **look at the image** before placing anything.

## Geometry

- Scene grid must match the art: count squares on the map, and set width/height/grid size so one square is one grid cell. Check with a screenshot.
- Canvas coordinates include scene padding. With `backgroundMapping` from generate-scene/apply-scene-candidate:
  `canvasX = sceneX + imageX × sceneWidth / imageWidth` (same for Y). Apply the padding once, not twice.
- Walls: a few long segments along real obstructions beat pixel tracing. Add doors where the map has doors, and leave real exits open. Water and difficult terrain are not walls.
- Verify by setting darkness, placing a light, and taking `get-screenshot`: light should stop at walls.

## Light and vision

- Daylight outdoors: enable global illumination (`update-scene { "environment.globalLight.enabled": true }`).
- Dark interiors: darkness plus placed lights (torches, braziers). Give PCs the vision their sheets say they have.
- Never "fix" a black screen by turning off token vision or resetting fog. That reveals what the GM hid. Check the token's sight settings and the scene's light instead.
- `get-screenshot` shows the **GM's** view. To prove what a player sees, view the scene as that player or ask the GM to check.

## Actors and tokens

- Reuse existing actors and compendium entries (`search-compendium`, `create-character-from-compendium`). On dnd5e the SRD covers most monsters. Check the adventure's statblock against the compendium one, and record any difference as an adaptation or fix it.
- Custom creatures, sidekicks and pregens: build them from the **source's** stat block with `create-character`, then **read the computed sheet back against the printed block, number by number**. In the live test this caught 13 mismatches on two creatures. Keep the original block on its Source page. dnd5e 6 traps:
  - A weapon created from scratch through `create-character` gets **no attack activity**, so it can't attack. Import a compendium weapon and edit it, or add one with `update-character-item { "system.activities.dnd5eactivity000": {"_id":"dnd5eactivity000","type":"attack","activation":{"type":"action","value":1},"attack":{"type":{"value":"melee","classification":"weapon"}},"damage":{"includeBase":true,"parts":[]}} }`.
  - Attack bonuses live on the activity (`system.activities.<id>.attack.bonus`), not on the weapon.
  - NPC skills have no check-bonus field, so a write to `skills.<key>.bonuses.check` is silently dropped. Printed sidekick blocks often use their own maths. Use an Active Effect, or record the printed numbers in the actor's biography and in Adaptations.
  - **Familiar's `resolve-multiattack` builds the attack list by parsing the Multiattack text.** Put the version the creature uses in this fight first ("makes three attacks: one with its bite and two with its claws"), and give weapons plain names that match those words ("Bite", "Claw"). In the live fight, a form-dependent text plus names like "Claw (Hybrid Form Only)" produced bite + bite. After the fix it produced bite + claw + claw. Test one `resolve-multiattack` per monster (with `force` and a reason, outside a real fight) before play.
  - Set each skill's `ability` explicitly when you create it. Skills given only a proficiency value came out computed from DEX (Athletics, Intimidation and Deception were all wrong).
  - Imported compendium gear may follow different rules from the book (a 1d6 slashing handaxe where the block says 1d8 piercing). The book wins.
- The player's character: read their sheet too, before the first fight. A dnd5e character made without a species has **walk speed 0** and shows as "slowed". In testing, a cold-start playing AI spotted this; the preparing pass had missed it. Then assign ownership (`assign-character-ownership`, level 3 = owner). A companion the player controls gets owner permission too. A GM-run companion doesn't.
- Token settings: disposition (friendly/neutral/hostile), linked for unique NPCs and unlinked for generic monsters, vision for PCs, size from the stat block. Portrait and token art are separate fields.
- Names are visible. Use player-safe names for tokens of unrevealed creatures ("Hooded figure", not "The Traitor").
- NPCs who talk a lot: `set-table-npc` lets players address them with `@Name` in Foundry chat. Familiar never reads their biography field, so private notes can go there.

## Staging, not starting

- Create scenes inactive (no `activate`) and set `navigation: false` with `update-scene`. Familiar can't put scenes into folders: `update-scene` strips `folder`.
- **Walls, lights, tokens and their list/delete tools all act on the active scene only.** To build walls and lights for a future map, activate it briefly at a moment the GM confirms nobody is playing, build and verify, then switch back to the scene the players should be on. Tested live: 23 cave walls, then a torch in darkness seen from the player's token. Light stopped at the walls. The GM's own screenshot ignored the darkness entirely.
- **Tokens follow the same rule**, so you can't stage tokens on an inactive scene through Familiar without pulling the players there. Instead, write the placement into the Prep page as **canvas pixel positions** ("Kestrel x=660 y=1320; Thalgar x=720 y=1440; Deep Scion ×2 at x=780 y=1560 and x=900 y=1680, hostile"). The runner places them right after `switch-scene` on arrival. The GM can also drag tokens in by hand ahead of time.
- Compute positions from the live scene rectangle: Foundry pads scenes (25% by default, rounded up to whole squares), so a 1500 px map with 60 px squares starts at canvas (420, 420). Then `canvas = sceneX + square × gridSize`. Get `sceneX/sceneY` from `generate-scene`'s `backgroundMapping`, or by reading the scene in the browser.
- Record each scene's name and ID, its Source page, its token placement and its entry point in Prep, and list the scenes in the Runner Guide's *Where things are* section.

## Audio (if the table wants it)

Use the GM's playlists or permitted packs (`search-playlists`, `search-ambience`). Stage, don't play: note in Prep which ambience fits which scene, and let the runner start it on arrival. Avoid overlapping loops. Generated sound effects post a public chat card describing them, so keep them spoiler-free.

## Scene readiness record

For each scene, note in Prep: art source, grid matches? walls/doors? lights/darkness? tokens placed (hidden where needed)? player visibility checked? audio staged? Mark parts the table doesn't use as *n/a*. Don't build walls or grids just to tick a box.
