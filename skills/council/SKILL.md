---
name: council
description: Convene the PolyClaude council, a panel of distinct perspectives (six built-in cognitive lenses, or an expert panel the user names). Members analyze a question independently, cross-examine each other's arguments, and are synthesized into a decision report. Use when the user asks to "run it through the council", "spin up the council", or "convene a panel of experts", wants "multiple perspectives" or "diverse viewpoints", a "devil's advocate", to "red team this", "stress test this idea", or "poke holes in this", or wants a plan, decision, or idea weighed from many angles before committing. The whole council runs inside this session as subagents; it is not a live room with other agent runtimes.
version: 0.3.0
---

# PolyClaude Council

Convene a council of distinct minds. Each one thinks alone first, then they read and answer each other. Then you synthesize what survived into a decision the user can act on.

**Arguments (may be empty):** $ARGUMENTS

The question is in the arguments. If they're empty, it's whatever the user asked the council to consider in this conversation. If there's no clear question, ask for one before convening.

The reference files are in `${CLAUDE_SKILL_DIR}/references/`, the `references` folder beside this file. Read each one at the step that calls for it.

---

## 1. Parse flags

Flags can appear anywhere in the arguments. Remove them from the question.

| Flag | Effect |
|---|---|
| *(none)* | 4 members, with cross-examination (the default) |
| `--quick` | 2 members, independent round only (a fast sanity check) |
| `--full` | 6 members, with cross-examination |
| `--council N` | exactly N members (2–6), with cross-examination |
| `--deep` | members run on Opus instead of Sonnet |
| `--no-debate` | skip cross-examination at any size |
| `--include a,b` | seat these built-in lenses |
| `--exclude a,b` | keep these built-in lenses out |
| `--panel "role; role; …"` | staff the council with these experts instead of the built-in lenses |

Built-in lens names: `architect`, `skeptic`, `pragmatist`, `innovator`, `advocate`, `temporal`.

**Named experts count as a panel.** The user may describe who they want on the council: "a master typographer and a veteran interface engineer", "world-class memory researchers", "make the entities experts in X". Treat that exactly like `--panel`. Seat the council they asked for; don't map it back onto the built-in lenses.

## 2. Write the brief

Members are subagents. They can't see this conversation, so everything they need goes into one shared brief of about 150–400 words:

- **Question:** the decision or problem, stated precisely.
- **Context:** what's being built, what's already decided or tried, constraints, stakes, and what a good outcome looks like to the user.
- **Materials:** absolute paths or URLs of files and documents worth reading. Members may read them.
- **Scope:** what's in bounds, and what the user most wants from the council.

Use only what you actually know. When a load-bearing fact is missing, put it in the brief as an open question rather than inventing it. Every member gets the identical brief, so their differences come from their lens, not from what they were told.

## 3. Compose the council

- **Built-in lenses** (the default): read `${CLAUDE_SKILL_DIR}/references/classification.md`. Classify the question and seat members by its selection algorithm. Exclusions come out *before* seats are filled, so an excluded lens's seat goes to the next one in relevance order.
- **Expert panel** (`--panel`, or the user named the members): read `${CLAUDE_SKILL_DIR}/references/expert-panels.md` and write a full member card for each expert. `--include` can seat built-in lenses beside them.

Either way the council has 2–6 members. Someone speaks for the people the decision lands on: the User Advocate, or an expert who explicitly carries that role. Only `--exclude advocate` removes it.

## 4. Announce

The message that launches round 1 opens with this block, written as text before its Agent calls. A council takes minutes, and this is how the user knows who is deliberating and what it will cost while they wait. Never launch members without it, including when you arrived here through the `/polyclaude` command.

```
Convening the PolyClaude Council…

Question type: [category, or "Expert panel"]
Council ([N]): [Member] · [Member] · …
Rounds: independent → cross-examination (skip it with --no-debate)   [or: independent only]
Mode: Sonnet   [or: Deep (Opus)]
Estimated: ~$[low]–[high] at API prices · ~[N] min
```

Estimates come from measured runs with an Opus main session and Sonnet members:

| Council | Whole council | Typical time |
|---|---|---|
| 2 members, one round (`--quick`) | ~$0.70–1.10 | ~3 min |
| 4 members, one round (`--no-debate`) | ~$1.00–1.50 | ~5 min |
| 3–4 members with cross-examination | ~$2.00–3.00 | ~8–10 min |
| 6 members with cross-examination (`--full`) | ~$3.50–5.00 | ~12–15 min |

Most of the cost (about 80%) is the main session running the council. `--deep` members cost about 2.5× Sonnet members, which adds roughly a third to the total. With a Sonnet main session, the whole council costs roughly half. On a Claude subscription, this comes out of usage limits rather than dollars.

## 5. Round 1: independent analysis

Built-in members take their cards from `${CLAUDE_SKILL_DIR}/references/perspectives.md`. Expert members take the cards you wrote in step 3.

Launch **every member in a single message**, the one that opens with the announce block. Use one Agent call per member, so they run in parallel and nobody anchors on anybody else. Each call uses:

- `subagent_type: polyclaude:council-member`, this plugin's read-only member. It can read, search, and browse, but it can't modify anything. If that agent type isn't available, use `general-purpose`; the prompt already forbids changes.
- `model: sonnet` (or `opus` with `--deep`).
- `description: "Council: [Member]"`.

Use this prompt:

```
You are [MEMBER] on a PolyClaude Council: [N] members analyzing one question from deliberately different angles. The others: [each other member, with their lens in a few words].

[MEMBER CARD: identity, methodology, signature questions, challenge targets, confidence calibration, output structure]

---
BRIEF
[the shared brief]
---

INSTRUCTIONS
1. Work through your methodology step by step, applied to this situation rather than in general.
2. Ground your claims in the brief. Read any listed file or URL you need, and look further only if a load-bearing fact is unclear. Never create, edit, or delete anything.
3. Say where you expect the other members to disagree with you, and why you'd still hold your view.
4. Rate your confidence (High / Medium / Low) with a specific reason. Name what you're most and least qualified to judge.
5. Use your output structure. 300–600 words.
6. End with a **Claims** block: your 3–5 load-bearing claims, numbered, each one sentence with its key reason. In round 2 the other members cross-examine these, so make each one specific enough to disagree with.
```

## 6. Round 2: cross-examination

Skip this round for `--quick` or `--no-debate`. Otherwise, read `${CLAUDE_SKILL_DIR}/references/cross-examination.md`. Launch every member again, in one message. Each one holds its own round-1 analysis and the other members' Claims blocks, all verbatim. Members quote what they challenge, concede what landed, and say whether their position held, sharpened, or changed.

## 7. Synthesize

Read `${CLAUDE_SKILL_DIR}/references/synthesis.md` and do the synthesis yourself. Don't spawn an agent for it. When cross-examination ran, give the most weight to what survived it: consensus that held under challenge, positions that changed, and tensions that persisted after direct engagement.

## 8. Report

Read `${CLAUDE_SKILL_DIR}/references/output-format.md` and write the Council Report. Show the user only the report. Raw member output goes inside its collapsible sections.

---

## Ground rules

- Let the members speak. Don't editorialize between rounds.
- If a member fails or times out, go on without them and name the gap in the report ("The Skeptic was unavailable for this council.").
- Represent tensions faithfully. A disagreement that survived cross-examination is information, not noise to smooth over.
- The council advises. Members never modify files. If the user wants the verdict acted on, that happens after the report, in the main session.
