# Finding an adventure

Use this when the user says something like "find me a one-player D&D adventure and set it up" and supplies nothing. If they'd rather have an original adventure written for them ("write me a pirate campaign", "teach my kid fractions with dragons"), that's the companion skill [Campaign Writer](https://github.com/cha1latte/campaign-writer); its package then installs through this skill. The job is to find a **legitimately available** adventure that fits *their* table, get their pick, then install it.

## 1. Read the table before searching

Get these from the world and the request. Ask only for what you can't infer, and keep it to one short question at most:

| Need | Where to look first |
|---|---|
| Game system and edition | `get-world-info` → `system`; existing actors |
| Number of players; solo or group | Connected users, owned characters, the request ("one player") |
| Character level, or "new character" | Owned actors' levels |
| Length: one-shot, a few sessions, campaign | The request; default to a one-shot or short adventure for a first install |
| Tone and content limits | Table Rules, the Table Profile page, earlier play |
| Maps or theatre-of-the-mind | Existing scenes and the table's preferences |

## 2. Search legitimate sources

Prefer, in order:

1. **What the user already owns.** Check their Downloads, documents and Foundry compendiums/modules (`list-compendium-packs`). Installed adventure modules beat new downloads.
2. **Official free releases** from the game's publisher, such as free adventures in the publisher's own magazine or site.
3. **Creator-published free or pay-what-you-want adventures** on the creator's storefront (DMs Guild, DriveThruRPG, itch.io). A free account login is the user's job. Don't create accounts or enter payment details for them.
4. **Openly licensed adventures** (Creative Commons, ORC, OGL). Note the licence's terms.

Never use pirated copies, file-sharing mirrors or scraped "free PDF" sites, even if a search engine shows them first. If the only copy you can find is on such a site, say so and send the user to the legitimate store page instead.

For solo play, look for these labels: "solo", "DM-free", "duet" (one GM and one player), "gamebook", or a party adventure that scales down. A party adventure can work solo with a sidekick or companion, but flag that it'll need balancing.

## 3. Present a short list

Give **two or three** options. They're the user's to choose, so keep this spoiler-free:

```text
1. <Title> (<publisher>, <licence/price>): <one-line hook, no twists>
   Fits: <system/edition>, <levels>, <solo/duet/party>, <length>, <maps included?>
   Watch out: <needs level-7 character / gamebook format / login to download>
```

Don't download or install until they pick, unless they said "just pick one". In that case pick the best fit and say why in one line.

## 4. Fetch and record provenance

Download into a working folder the user can see (not the Foundry data folder). Record the following in the **Source** journal's first page:

- title, author, publisher, version or issue, and the URL it came from
- licence or permission text (for example "personal use only")
- page count, and whether maps, stat blocks, pregens and handouts are included

Then continue with [Source intake](source-intake.md).

## Format notes that change the install

| Format | What it means for setup |
|---|---|
| **Gamebook / DM-free** (numbered entries, "turn to 23") | Store entries in order with their numbers. The playing AI narrates an entry, offers its listed choices, and tracks codewords and flags in *Trackers*. Off-script player ideas still deserve a fair ruling. Record each such ruling in *Adaptations*. |
| **Duet** (one GM, one player) | Usually ready as-is. Check for companion NPCs who need actor sheets. |
| **Party adventure run solo** | Plan a sidekick, companion NPC or level adjustment. Record it in *Adaptations* as a deliberate change. Check every fight for a solo character's action economy. |
| **Foundry adventure module** | Import its compendium content (scenes, actors, journals) instead of rebuilding it. Still add Start Here, Prep and Table Rules. |
| **Notes or homebrew** | Treat the user's notes as the source. Label anything new you author as new. |
| **A Campaign Writer package** (has `campaign.json` and `foundry/handoff.md`) | Follow its `foundry/handoff.md`. Its book chapters are the Source; push the ready-made pages in `foundry/pages.json` instead of converting the markdown yourself (tables are already lists; read-aloud is labelled). Copy `build/maps/*-vtt.png` into Foundry's Data folder and create walls from `*-walls.json`. Teach-mode packages add Table Rules lines, a Runner Guide section and a *6 Learning Tracker* page to Start Here. |
