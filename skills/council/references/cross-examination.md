# Cross-Examination

Round 1 produced independent analyses. Round 2 makes the council an actual council: each member reads the others and answers them.

## Why independent first

Members answer alone in round 1 so nobody anchors on anybody else. Cross-examination then tests those positions against each other. Keep that order.

## What each member receives

- its identity and challenge targets,
- the brief,
- its own full round-1 analysis,
- every other member's **Claims** block, Recommendation, and Confidence from round 1.

Pass all of it verbatim, in the members' own words. Never pass a summary you wrote.

The other members get Claims blocks rather than full analyses because you retype everything you pass. Full analyses for everyone grows with the square of the council's size, and a four-member council would spend minutes on copying alone. The Claims block is each member's own statement of what's load-bearing, so nothing is paraphrased. Your synthesis still works from the full analyses.

If a member left out its Claims block, pass its Recommendation plus its single most load-bearing paragraph, verbatim.

## Launching round 2

Use one message, with one Agent call per member. Use the same `subagent_type` and `model` as round 1, and `description: "Cross-exam: [Member]"`.

### Prompt template

```
You are [MEMBER] on a PolyClaude Council. In round 1 you and [N−1] other members analyzed this question independently. Now you've read each other.

YOUR LENS
[identity + challenge targets]

BRIEF
[the shared brief]

YOUR ROUND-1 ANALYSIS
[verbatim]

THE OTHER MEMBERS' CLAIMS (in their own words)
### [Member A]
Recommendation: [verbatim]
Confidence: [verbatim]
[Claims block, verbatim]
### [Member B]
…

CROSS-EXAMINATION: answer in this structure, 200–400 words.

**Strongest point from another member:** Who, what, and what it changes or sharpens for you.
**Challenges:** The 1–2 claims you most disagree with. Quote each briefly and name who made it. Give your reason, and name the fact, test, or scenario that would settle it.
**Concessions:** What in your own round-1 analysis you now think was wrong, overweighted, or missing. "None" is allowed only if you say why nothing landed.
**Position:** HELD, SHARPENED, or CHANGED, then your recommendation now in 1–2 sentences.
  - HELD: your recommendation stands as it was, and you've said why the challenges didn't move it.
  - SHARPENED: the same direction, but now narrower, conditional, or re-reasoned. Name what specifically changed.
  - CHANGED: you now recommend something different. Name what moved you.
  Pick the one that's true, not the one that sounds balanced.
**Confidence:** High / Medium / Low, with the reason. Say if it moved.

Engage the actual claims; don't restate your round-1 analysis. Don't agree just to be agreeable, and don't dig in to save face. Update where the evidence moved you, and hold where it didn't.
```

## What to keep for synthesis

For each member, note:

- position status (held, sharpened, or changed),
- their concessions,
- each challenge: who challenged whom, the claim, and the test that would settle it.

These feed the cross-examination signals in `synthesis.md`.
