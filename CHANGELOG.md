# Changelog

## 2.2.2 (2026-10-07)

- Familiar 2.26's **Automatic** image, music, sound-effect and playlist modes are permission for the AI GM, not triggers. An outside assistant (Claude, Codex) sees them only in `get-world-info`, and in a live session it ignored them: no picture, sound or new music unless the player asked. New section in `familiar-facts.md`, and a Media line in the Runner Guide template telling the runner to act on whatever is set to Automatic.

## 2.2.1 (2026-10-02)

- Installing a Campaign Writer package now says to follow its Teach style. Campaign Writer 1.1 packages come in **Stealth** (the player must never feel taught: GM-side Learning Tracker, no out-of-character debrief, an opt-in decoder) and **Open** (classroom) versions of the Table Rules and Runner Guide, and the installer should use the one the package ships.

## 2.2.0 (2026-10-01)

**New**
- Table chat keeps its own turn history, separate from the GM's chat. Switching a world to another campaign now has a tested recipe: an `ACTIVE CAMPAIGN` first line in the Table Rules, then Table Chat off and on. *New chat* in the GM panel doesn't reset table chat. Found in a live test.
- Installs packages from the companion skill [Campaign Writer](https://github.com/cha1latte/campaign-writer): its `foundry/handoff.md` and ready-made `foundry/pages.json`, including Teach-mode Table Rules and a Learning Tracker page.
- Finding adventures points to Campaign Writer when the user wants an original adventure instead of a published one.

## 2.1.0 (2026-10-01)

A full rebuild of the v1 portable skill, tested live in Foundry VTT 14 with Familiar 2.25.

**New**
- One 8-step workflow with a "pick the job" table: find, install, prep next session, closeout, resume, scene work, drift audit.
- `familiar-facts.md`: Familiar's real limits and gotchas, verified live. The 3,000-character Table Rules cap; GM-only journals; canvas tools that only touch the active scene; what solo play needs; tool receipts; disconnects.
- Finding adventures from legitimate sources, including gamebook, duet and party-run-solo formats.
- Faithful PDF import: column- and font-aware extraction, page-image checks, push-by-script, and a byte-level read-back diff.
- Journal layout with templates: a Runner Guide that carries the procedure into Foundry, plus Pickup, Table Profile, Adaptations, Trackers and Session Log.
- A compact Table Rules block whose every line came from a failure seen in testing.
- A handoff test that uses Familiar's GM-only receipts as evidence.
- A one-write closeout that the AI GM actually completes; the preparing assistant files the log at the next prep.
- dnd5e actor traps: dropped skill bonuses, weapons with no attack activity, multiattack parsing, speed 0 without a species.
- A full worked example of the live test.

**Tested**
- Two rounds of live tests: install, play, closeout, resume, the first fight, walls, lighting and audio. Also an outside AI as GM, a cold preparing AI given only the skill, and a second player.

## 1.0 (2026-09-25)

The first portable edition: principles for source fidelity, runner handoff, continuity and verification.
