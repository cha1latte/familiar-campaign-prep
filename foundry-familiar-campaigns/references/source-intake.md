# Source intake and drift audit

The playing AI can only be faithful to text it can read. Put the original in Foundry, section by section, and check the copy against the original.

## Inventory

Before extracting, list:

- the file(s), edition or version, page count, and whether pages are printed with numbers that differ from the PDF index
- sections or numbered entries, and where the adventure begins
- maps, handouts, tables, stat blocks, pregens or sidekicks, codewords, and appendices
- anything unreadable (scanned pages, text baked into images)

Record anything you skip, and why. "Imported the first chapter" is fine. Calling a partial import "the adventure" is not.

## Extract with the layout in mind

- **Two-column pages** interleave lines under naive extraction ("Welcome to Frozen Offerings, a solo (DM free) adventure your choices affect the outcomes…"). Extract by column instead. For example, crop each page's left and right halves with `pdfplumber` (`page.crop(bbox).extract_text()`), or use `pdftotext -layout`, then fix the reading order.
- **Doubled heading letters** ("FFRROOZZEENN") come from faux-bold rendering. Use `page.dedupe_chars()` in `pdfplumber`, or collapse doubled characters in headings only.
- **Stat blocks, tables, sidebars** often come out jumbled. Re-key them by hand from the page image, or reconstruct them carefully. Never guess a missing number.
- **Text inside images** (maps, handouts, combat sheets) needs looking at. Render the page to an image and read it.
- Fix hyphenation and page furniture (running headers, page numbers, licence footers). Keep everything else.

**Check the extraction.** For every section you'll use in the next session, compare the extracted text with the page image: reading order, numbers (DCs, HP, distances, quantities, times of day), names and choices. Spot-check the rest, and record which sections were checked by eye and which were only sampled. Text equality doesn't prove that layout, image content or section boundaries came through.

**A Campaign Writer package** needs no extraction: its `book/*.md` chapters are the original text, and `foundry/pages.json` already holds them as Foundry-ready pages (one per node, tables as one line per row, read-aloud labelled). Push those pages as they are and read them back.

## Store it in Foundry

One **Source** journal per adventure. One page per section, or per small run of numbered entries, kept well under the 50,000-character page limit (aim for 3,000–15,000 characters so a single read stays cheap). Name pages so they sort and can be found:

```text
00 About this source          ← provenance, licence, inventory, extraction notes
01 Introduction (pp. 1-2)
02 Entries 1-12 (pp. 3-5)
...
20 Thalgar sidekick stat block (p. 22)
```

**Getting long text in without retyping it.** An AI copying 100,000 characters through its own tool calls is slow and can drift. Choose, in order:

1. If you can run scripts and a browser on the GM's machine, push the extracted pages straight into Foundry (for example, a separate Assistant-GM login with Playwright calling `JournalEntryPage.create`). Block Familiar's module scripts in that tab so it doesn't compete with the GM's tab for the bridge.
2. Otherwise use `create-journal-page`, one page per call, copied from the extraction file, never retyped from memory.
3. Either way, **read every page back and compare it with the extraction file by script**. In the live test, all 14 pages, both scripted and hand-pushed, matched exactly; the check is what lets you say so.

Head each page with a source line and a status line:

```html
<p><em>Source: Frozen Offerings, Dragon+ 34, pp. 3-5. Original text; do not paraphrase. Checked against page images: yes (2026-10-01).</em></p>
```

Then the text, with minimal HTML: paragraphs, `<h3>` for entry numbers, `<strong>` for names of choices and codewords. Keep the source's wording. Turn source tables into one line per row ("Snowshoes, two pairs: 10 gp"), because tables reach the runner as run-together text.

Keep GM-only content (answers, secrets, monster tactics) in Source and Prep. Player handouts go in a separate *Handouts* journal the GM can share.

## Prep is derived, and marked as derived

Prep pages (see [Session prep](session-prep.md)) summarise and organise. Each prep item names its Source page. Facts that constrain play come over **exactly**: time of day, travel times, quantities, DCs, NPC knowledge and motives, clue locations, rewards, and conditions for success and failure. When prep and source disagree, the source wins unless the *Adaptations* page records the change.

## Adaptations ledger

Every intentional change gets an entry: what the original says, what we're doing instead, why, and who approved it. Examples include solo scaling, edition conversion, a missing stat block filled from the SRD, or a house rule. Write it as a list, not an HTML table. Familiar's `get-journal` returns plain text, and table cells run together into mush ("#Source saysWe do insteadWhy…").

```text
1. Source (p. 2): DM-free, the player rolls for everyone.
   We do: the AI GM rolls for monsters; the player still controls Thalgar. Why: an AI GM runs it. Approved: GM.
2. Source (p. 2): uses Sidekicks Essentials.
   We do: Thalgar's printed block, as-is. Why: supplement not owned. Approved: GM.
```

Separate four things: the publisher's own alternatives, deliberate adaptations, connecting detail you invented (label it), and contradictions inside the source. Contradictions need a ruling, not a quiet pick. Before calling something a contradiction, search the whole source for both terms. In *Frozen Offerings* the entries name the dragon "Angnath (she)" and the combat sheets "Xakkrath (he)". It looks like an error, but one entry reveals Xakkrath is Angnath's son: a twist, and one the runner must not spoil.

## Drift audit

When the playing AI said something that doesn't match the adventure, trace one fact through every layer and find the **first** layer where it went wrong:

```text
Original page → Source journal page → Prep page → Table Rules / Runner Guide → what the runner actually read (tool log) → its reply → saved state
```

| Where it broke | Typical cause | Fix |
|---|---|---|
| Source journal | Extraction error, missing section | Re-extract and re-check that section |
| Prep | Summary dropped or changed the fact | Fix the prep and add the exact fact |
| Rules / Runner Guide | The runner wasn't told to read the source | Tighten the read rule |
| Retrieval | The runner never called `get-journal-page`; it relied on excerpts | Fix the pointer and test with a fresh chat |
| Reply | Read correctly, invented anyway | Note it. Flag it to the GM; adjust the rules wording only if it repeats |
| Saved state | Narrated but not saved, or saved twice | Repair from the chat record; never replay the event |

Example: the source says dusk, the Source page says dusk, the Prep says "a sunny day", and the runner narrates morning. The import is fine. The prep lost the fact, and the runner leaned on the prep. Fix the prep line and leave the import alone.

Check correct facts too: the audit should report what survived, not only what broke. Don't blame the model or Familiar without the tool log to show it.

Already-played events stay played. If play contradicted the source, record it in the Session Log as what happened, flag it to the GM, and decide together whether it becomes an adaptation. Don't quietly retcon.
