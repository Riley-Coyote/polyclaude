---
name: council
description: This skill should be used when the user asks about "multi-perspective analysis", "multiple viewpoints", "council deliberation", "dialectical synthesis", "different perspectives on", "diverse viewpoints", "devil's advocate", "red team this", "stress test this idea", "poke holes in this", or wants to analyze a question from multiple cognitive angles simultaneously.
version: 0.2.0
---

# PolyClaude Council — Multi-Perspective Analysis

PolyClaude spawns parallel cognitive perspectives to analyze questions, plans, and ideas from multiple angles, then synthesizes findings through structured dialectical analysis.

## When to Use

The `/polyclaude` command is valuable when:
- Making a significant decision and wanting to stress-test it before committing
- Evaluating a plan or proposal from angles you might not think of alone
- Exploring a problem space before narrowing to a solution
- Wanting a "red team" analysis of an idea
- Needing to present a balanced assessment to stakeholders

## The Council

Six cognitive perspectives are available. The **User Advocate** is always included (unless explicitly excluded); others are selected adaptively based on the question type:

| Perspective | Lens | Always Asks |
|---|---|---|
| **User Advocate** (default) | Experience, empathy, accessibility | "How does this feel to encounter?" |
| **Architect** | Systems, structure, scalability | "What are the load-bearing assumptions?" |
| **Skeptic** | Risks, assumptions, failure modes | "What are we not seeing?" |
| **Pragmatist** | Simplicity, effort, trade-offs | "What's the simplest thing that works?" |
| **Innovator** | Alternatives, inversions, creativity | "What would the opposite look like?" |
| **Temporal Analyst** | Timelines, sequencing, second-order effects | "What does this look like in 6 months?" |

## How to Use

### Basic
```
/polyclaude Should we rewrite the auth system or patch it?
```

### Council Size
```
/polyclaude --quick Is this API design reasonable?          # 2 perspectives — fast sanity check
/polyclaude Should we use Redis or Postgres for sessions?   # 4 perspectives — default
/polyclaude --full What's our mobile strategy?              # 6 perspectives — comprehensive
```

### Quality
```
/polyclaude --deep What architecture should we use?         # Opus agents — maximum depth
/polyclaude --full --deep Should we pivot to B2B?           # All 6 on Opus — thorough
```

### Perspective Control
```
/polyclaude --include temporal,innovator Should we refactor the API?
/polyclaude --exclude pragmatist What if we open-sourced this?
```

## Estimated Costs

| Mode | Perspectives | Sonnet (default) | Opus (--deep) |
|---|---|---|---|
| `--quick` | 2 | ~$0.15 | ~$0.75 |
| *(default)* | 4 | ~$0.30 | ~$1.50 |
| `--full` | 6 | ~$0.45 | ~$2.25 |

## What You Get

A **Council Report** with:
1. **Verdict** — Bottom-line recommendation
2. **Consensus Points** — Where a strong majority agrees (highest confidence)
3. **Key Tensions** — Structured disagreements with resolution proposals
4. **Blind Spots** — What no perspective addressed
5. **Confidence Map** — Visual certainty/uncertainty across the analysis
6. **Next Steps** — Ordered, actionable items
7. **Individual Perspectives** — Full analyses in collapsible sections

Quick councils (2 perspectives) use a compact format: Verdict, Agreement/Disagreement, Blind Spots, Next Steps.

## What Makes This Different

This is not "ask N AI agents the same question." It is structured dialectical analysis:
- Each perspective has a distinct **cognitive methodology** (not just a different prompt)
- Perspectives are instructed to **challenge each other's likely arguments**
- Synthesis identifies **tensions** (not just differences) and proposes **resolutions**
- **Blind spot detection** surfaces what nobody addressed
- Output is a **decision document**, not a discussion transcript
- Council composition **adapts to your question type** with named presets for size
