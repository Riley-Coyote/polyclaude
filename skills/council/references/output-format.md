# Council Report Output Format

Choose the format based on council size.

---

## Quick Format (2 perspectives)

Use this compact format for `--quick` councils.

```markdown
## Council Report: [Concise Title]

**Question:** [Original question, verbatim]
**Council:** [2 perspective names]
**Mode:** [Quick / Quick Deep]

---

### Verdict

[1-3 sentence bottom-line recommendation.]

---

### Analysis

**Agreement:**
[What both perspectives agree on]

**Disagreement:**
[Where they diverge, with each side's reasoning]

---

### Blind Spots

- [What neither perspective addressed]

---

### Recommended Next Steps

1. [Action 1]
2. [Action 2]

---

### Individual Perspectives

<details>
<summary>[Perspective 1]'s Analysis</summary>

[Full analysis]

</details>

<details>
<summary>[Perspective 2]'s Analysis</summary>

[Full analysis]

</details>
```

---

## Standard Format (3-6 perspectives)

Use this full format for default, `--full`, and custom council sizes.

```markdown
## Council Report: [Concise Title Derived from the Question]

**Question:** [The user's original question, verbatim]
**Council ([N] perspectives):** [Names of all selected perspectives]
**Mode:** [Default / Deep / Full / Full Deep]

---

### Verdict

[1-3 sentence bottom-line recommendation. Actionable. No hedging without specifying what the decision depends on. This is what someone reads if they only have 10 seconds.]

---

### Consensus Points

[Findings where a strong majority of perspectives agreed. Bulleted list. These are the safest bets.]

- [Point 1]
- [Point 2]
- [Point 3]

---

### Key Tensions

[For each meaningful disagreement between perspectives — limit to 2-3 most significant:]

**Tension: [Value A] vs. [Value B]**
- **[Perspective 1]** argues: [their position and reasoning]
- **[Perspective 2]** counters: [their position and reasoning]
- **Resolution:** [Synthesized recommendation, or "Genuine trade-off — choose based on whether you prioritize [X] or [Y]"]

[Repeat for each significant tension.]

---

### Blind Spots

[What no perspective addressed. These are not criticisms — they are unexplored territories that could change the recommendation.]

- [Blind spot 1]
- [Blind spot 2]

---

### Confidence Map

| Aspect | Confidence | Signal |
|--------|-----------|--------|
| [Aspect 1] | High / Medium / Low | [Why — e.g., "Strong majority agrees" or "Divergent views"] |
| [Aspect 2] | High / Medium / Low | [Why] |
| [Aspect 3] | High / Medium / Low | [Why] |

---

### Recommended Next Steps

1. [Most urgent / highest confidence action]
2. [Action that resolves the most uncertainty]
3. [Action informed by the key tension]
4. [Optional: longer-term action]

---

### Individual Perspectives

<details>
<summary>The User Advocate's Analysis</summary>

[Full analysis from the User Advocate agent]

</details>

[Repeat <details> block for each perspective in the council]
```

---

## Formatting Guidelines

- Use **bold** for perspective names, tension labels, and key terms
- Use tables for the confidence map (easy to scan)
- Use `<details>` tags for individual perspectives (keeps the report scannable while preserving full analysis)
- Keep the Verdict under 50 words
- Keep Consensus Points to 3-5 bullets max
- Keep Blind Spots to 2-4 items (quality over quantity)
- Next Steps should be concrete enough to act on immediately
- At 5-6 perspectives, do not list every individual disagreement — surface only structurally significant tensions
