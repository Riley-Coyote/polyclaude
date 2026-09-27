# Expert Panels

Use this when the user passes `--panel` or names the members they want. The goal is the panel they asked for, built with the same rigor as the built-in lenses, not six generic lenses wearing expert name tags.

## Seating the panel

- **Named roles** ("a master typographer; a 20-year interface engineer; a conversion lead"): seat one member per role, in the user's words.
- **A collective** ("world-class memory researchers", "a diverse set of experts on X"): design the members yourself. Seat 4 by default (2 with `--quick`, 6 with `--full`, N with `--council N`). Cover different sub-disciplines, and include at least one member likely to dissent from the obvious answer. A panel of five people who agree is one person.
- **More than 6 roles named:** merge the closest overlaps down to 6, and say which ones you merged.
- **Built-in lenses** named with `--include` (e.g. `--include skeptic`) sit beside the experts and count toward the 6.
- **The people affected keep a voice.** If no expert naturally speaks for the people the decision lands on, give that role explicitly to the closest expert ("…and the panel's user advocate"), or seat the built-in User Advocate. Skip this only with `--exclude advocate`.

## Writing a member card

Write each card in the same shape as `perspectives.md`, so every member gets equal depth:

```
## [ROLE NAME]

**Identity:** Who they are, and the specific experience that makes their judgment worth having: named domains, the kind of work they've shipped, what they've seen fail. 2–4 concrete sentences. Don't let superlatives stand in for substance.

**Methodology:** 4–6 steps this expert actually uses. These are the moves of their craft, specific to the discipline, not generic analysis steps. (A typographer checks measure, rhythm, and texture; a memory-systems engineer checks write paths, retrieval failure, and decay.)

**Signature Questions:** 3–4 questions only this expert would ask first.

**Challenge Targets:** 1–2 other members of THIS panel, by name, and where this expert will push back on them.

**Confidence Calibration:** Where their judgment is strong, and where it's weak and they should defer.

**Output Structure:** 3–5 headed sections that fit the discipline, ending with **Recommendation** (1–2 sentences) and **Confidence** (High / Medium / Low — reason).
```

## Distinctness check

Before launching, reread the cards side by side. If two members would reach the same conclusion by the same route, change one's methodology or replace them. Members should differ in method and values, not just in title.

## Announcing a panel

Give each member's lens in a few words:

```
Question type: Expert panel
Council (4): Master Typographer (type as voice and texture) · Veteran Interface Engineer (what survives real browsers) · …
```
