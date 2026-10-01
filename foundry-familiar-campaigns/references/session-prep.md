# Session prep

Prepare **the next stretch of play**, not the whole campaign: the section the Pickup points at, plus everything the party could plausibly reach next session. As a rule of thumb, go to the next natural stopping point beyond where a fast session would get, and include every fight on the way. If existing prep already covers that stretch, **audit it against the Source** and fix it, rather than rewriting it. Then extend it only as far as the rule above needs. Any fight in range needs its actors and scene built and number-checked now (see [Scene production](scene-production.md)); a fight with no actor isn't prepped. The test of good prep is whether another GM could run it **without inventing the answers**.

## Start from what actually happened

1. Read Start Here: the Pickup, the latest Session Log entry, Trackers and Adaptations.
2. Read the Source pages for the current and next sections. For a published adventure, prep is built *from* these pages, not from memory or an earlier summary.
3. Check live state: active scene, token positions, HP, items. Read sheets with `get-character { sections: ["all"] }`. The default read shows flat save bonuses and hides skills and resources, so you can't check proficiencies from it. Played results beat older notes. Older notes beat nothing. Flag any contradiction to the GM rather than picking a side silently.

## What a prep page must answer

For each place, scene or entry group (template: [Journal layout § Prep page](journal-layout.md#prep-page-skeleton)):

| Part | The runner must be able to answer… |
|---|---|
| **Situation** | What's going on here, and why, even if the PCs never show up? |
| **Place** | What's here to see, touch and use? What do searches find? Where are the exits? |
| **People** | What does each NPC want, know, not know, and say if asked? How do they react to help, pressure or attack? |
| **Discovery** | Which facts can the player learn here, and how? What do they lead to? |
| **Choices** | What can the player do? (The source's options first, then sensible others.) What does each cost or risk, and where does it lead? |
| **Resolution** | Which checks, DCs and outcomes the source gives, verbatim. What changes on success and on failure? |
| **Payoff** | What exactly do they gain (items, money, XP, codewords, goodwill), and when? |

Undefined detail that the runner will certainly need (the innkeeper's name, what's in the chest) should be written now and marked *new*. Don't leave it for the runner to improvise under pressure, and don't contradict the source while filling gaps.

## NPC knowledge cards

The single most common drift is an NPC knowing too much or too little. For each NPC who matters in this stretch:

```text
<Name> (actor id) | Source: <page>
Wants: <now>                     Fears/avoids: <…>
Knows (witnessed): <…>           Heard (rumour, may be wrong): <…>
Doesn't know: <…>                Believes wrongly: <…>
Will share: freely <…> / if persuaded <…> / never <…>
Voice: <2-3 words + one line of sample dialogue>
```

NPCs answer reasonable questions naturally. Not everyone is evasive or sinister. They can be wrong. They can't know what they never saw or heard.

## Mysteries and essential clues

If the adventure depends on the player learning something, list the conclusion first. Then give at least two (ideally three) **independent** ways to learn it: different places, people or methods. Three rolls against the same locked diary count as one route. Use the source's clues first. If you add a fallback, mark it as an adaptation. Never invent evidence that settles a question the source leaves open.

## Encounters: check the mechanics now, not mid-fight

Next to each encounter, write only the checks that apply:

```text
Creatures: <name ×n, actor ids, source/SRD stat block verified? y/n>
Edition: <2014 / 2024>; Familiar automation: <yes for dnd5e; manual otherwise>
Danger for THIS party: burst damage per round vs PC HP; what if the companion drops?
Tactics: <from source>; morale/retreat: <…>
Terrain/visibility: <light, cover, hazards>; timer or trigger: <…>
Special rules: <source exceptions, e.g. "no long rest unless the text allows">
Reward: <exact>
```

A solo PC is fragile. Check the enemies' best round of damage, not only XP budgets. Keep the source's difficulty unless the GM approves a change; record any change in Adaptations.

## Surprises, events and clocks (only if useful)

Use them when they make the world feel alive. Never add one to fill a quota.

- **Event:** what triggers it, the visible warning signs, how the player can prevent it, and when it expires. If prevented, it stays prevented. Don't relocate it to force it.
- **Clock:** what process it tracks, current/maximum, what advances it (and by how much), the signs players can see, and what happens when it fills. Questions, breaks and real-world time don't advance it unless the table agreed they do.
- Random picks use real dice (`roll-dice`) and record the method. Never claim a roll that didn't happen.

Store armed events and clocks in *Trackers* (GM-only).

## Review it by trying to run it

Before saving, walk five attempts through your notes, using only what's written:

1. The player follows the obvious lead.
2. The player tries something sensible the source didn't plan for.
3. The player fails a key roll, refuses, or retreats.
4. The player succeeds fast.
5. Someone else runs it cold.

Wherever you had to invent a central answer during the walk-through, add it to the prep (marked *new*) and walk it again. Reject prep that lists only intentions ("add a clue here"), hangs on one indispensable roll, or gives an NPC knowledge they couldn't have.

## Save and link

Write the prep pages to `<Title>: Prep <segment>`. Update the Pickup's *Read next* line to name them, and the Runner Guide's page index to list their IDs. Read everything back.
