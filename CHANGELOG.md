# Changelog

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
