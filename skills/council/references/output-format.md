# Council Report Output Format

Choose the format by council size.

---

## Compact Format (2 members)

Use this for 2-member councils.

```markdown
## Council Report: [Concise Title]

**Question:** [Original question, verbatim]
**Council:** [2 member names]
**Rounds:** [Independent only / Independent → Cross-examination]
**Mode:** [Quick / Quick Deep / Expert panel]

---

### Verdict

[1-3 sentence bottom-line recommendation.]

---

### Analysis

**Agreement:**
[What both members agree on]

**Disagreement:**
[Where they diverge, with each side's reasoning]

**Cross-examination:** *(only if round 2 ran)*
[What each conceded or challenged, and whether either moved]

---

### Blind Spots

- [What neither member addressed]

---

### Recommended Next Steps

1. [Action 1]
2. [Action 2]

---

### Individual Perspectives

<details>
<summary>[Member 1]</summary>

[Full analysis, then the cross-examination response if round 2 ran]

</details>

<details>
<summary>[Member 2]</summary>

[Full analysis, then the cross-examination response if round 2 ran]

</details>
```

---

## Standard Format (3–6 members)

Use this for default, `--full`, `--council N`, and expert-panel councils of 3 or more.

```markdown
## Council Report: [Concise Title Derived from the Question]

**Question:** [The user's original question, verbatim]
**Council ([N]):** [Names of all members]
**Rounds:** [Independent → Cross-examination / Independent only]
**Mode:** [Default / Deep / Full / Full Deep / Expert panel / Expert panel, Deep]

---

### Verdict

[1-3 sentence bottom-line recommendation. Actionable. No hedging without specifying what the decision depends on. This is what someone reads if they only have 10 seconds.]

---

### Consensus Points

[Findings where a strong majority agreed. These are the safest bets.]

- [Point 1] *(held under challenge)*
- [Point 2] *(unchallenged)*
- [Point 3]

---

### What Moved in Cross-Examination

[Only when round 2 ran. 2-4 bullets.]

- **[Member]** [sharpened / changed]: [from what, to what], moved by **[Member]**'s [argument].
- **Challenge that landed:** **[Member]** → **[Member]** on [claim]. [How it was answered.]
- **Held firm:** **[Member]** on [position], answering **[Member]**'s challenge with [reason].

[If nothing moved, say so in one line, and whether that reflects strong agreement or members talking past each other.]

---

### Key Tensions

[For each meaningful disagreement, up to the 2-3 most significant:]

**Tension: [Value A] vs. [Value B]**
- **[Member 1]** argues: [their position and reasoning]
- **[Member 2]** counters: [their position and reasoning]
- **In cross-examination:** [what they said to each other; whether the tension persisted, narrowed, or dissolved]
- **Resolution:** [Synthesized recommendation, or "Genuine trade-off: choose based on whether you prioritize [X] or [Y]"]

[Repeat for each significant tension.]

---

### Blind Spots

[What no member addressed. These are not criticisms. They are unexplored territory that could change the recommendation.]

- [Blind spot 1]
- [Blind spot 2]

---

### Confidence Map

*Members' own confidence, aggregated. This is self-rated, not measured calibration.*

| Aspect | Confidence | Signal |
|--------|-----------|--------|
| [Aspect 1] | High / Medium / Low | [Why, e.g. "Held under challenge" or "Disagreement persisted"] |
| [Aspect 2] | High / Medium / Low | [Why] |
| [Aspect 3] | High / Medium / Low | [Why] |

---

### Recommended Next Steps

1. [Most urgent / highest-confidence action]
2. [Action that resolves the most uncertainty, e.g. a settling test from cross-examination]
3. [Action informed by the key tension]
4. [Optional: longer-term action]

---

### Individual Perspectives

<details>
<summary>[Member]: [HELD / SHARPENED / CHANGED]</summary>

**Round 1**

[Full analysis from this member]

**Cross-examination**

[Full round-2 response from this member]

</details>

[Repeat the <details> block for each member. Without round 2, drop the status and the Cross-examination part.]
```

---

## Formatting Guidelines

- Use **bold** for member names, tension labels, and key terms.
- Use tables for the confidence map, because they're easy to scan.
- Use `<details>` tags for individual perspectives. That keeps the report scannable while preserving the full analyses.
- Keep the Verdict under 50 words.
- Keep Consensus Points to 3–5 bullets, What Moved to 2–4, and Blind Spots to 2–4.
- Next Steps should be concrete enough to act on immediately.
- At 5–6 members, don't list every individual disagreement. Surface only the structurally significant tensions.
