---
description: Analyze a question, plan, or idea from multiple cognitive perspectives simultaneously
argument-hint: [--quick|--full] [--deep] [--include name,name] [--exclude name,name] <question>
---

# PolyClaude Council

You are orchestrating a multi-perspective council analysis. Your job is to parse flags, classify the question, select perspectives, spawn parallel agents, and synthesize results into a Council Report.

## Step 1: Parse Input

The user's input is in `$ARGUMENTS`. Parse the following flags (order-independent, remove each from the question after parsing):

**Council size (mutually exclusive):**
- `--quick` → 2 perspectives (User Advocate + 1 adaptive)
- *(no flag)* → 4 perspectives (User Advocate + 3 adaptive) — the default
- `--full` → all 6 built-in perspectives
- `--council N` → exactly N perspectives, where N is 2-6 (power user override)

**Quality:**
- `--deep` → agents use `model: opus` instead of `model: sonnet`

**Perspective overrides:**
- `--include name,name` → force-include these perspectives regardless of classification (e.g., `--include temporal,innovator`)
- `--exclude name,name` → force-exclude these perspectives from the council (e.g., `--exclude pragmatist`)

Perspective names are lowercase: `architect`, `skeptic`, `pragmatist`, `innovator`, `advocate`, `temporal`

**Flag combinations are valid:** `--full --deep`, `--quick --include skeptic`, `--exclude architect --deep`, etc.

If `$ARGUMENTS` is empty or contains only flags with no question, ask the user what they'd like the council to analyze.

## Step 2: Determine Council Composition

Read the classification rules:
@${CLAUDE_PLUGIN_ROOT}/skills/council/references/classification.md

**Resolution order:**
1. Classify the question to get the default perspective set for the determined council size
2. Apply `--include` overrides (add perspectives, may increase council size up to 6)
3. Apply `--exclude` overrides (remove perspectives)
4. User Advocate remains unless explicitly excluded via `--exclude advocate`
5. Final council size must be 2-6 perspectives

## Step 3: Announce and Estimate Cost

Announce the council selection and estimated cost to the user:

```
Convening the PolyClaude Council...

Question type: [classification]
Council ([N] perspectives): [Perspective 1] + [Perspective 2] + ...
Mode: [Default (sonnet) / Deep (opus)]
Estimated cost: ~$[X.XX]
```

**Cost estimation table:**
| Perspectives | Sonnet (default) | Opus (--deep) |
|---|---|---|
| 2 (--quick) | ~$0.15 | ~$0.75 |
| 3 | ~$0.22 | ~$1.10 |
| 4 (default) | ~$0.30 | ~$1.50 |
| 5 | ~$0.37 | ~$1.85 |
| 6 (--full) | ~$0.45 | ~$2.25 |

## Step 4: Load Perspectives

Read the perspective definitions for the selected perspectives:
@${CLAUDE_PLUGIN_ROOT}/skills/council/references/perspectives.md

## Step 5: Spawn Parallel Agents

Launch **exactly N Agent tool calls in a single message** (where N is the resolved council size). This triggers parallel execution. Each agent must:
- Receive the full perspective identity, methodology, and output structure from `perspectives.md`
- Receive the user's question verbatim
- Be instructed to follow their perspective's methodology step by step
- Be instructed to rate their confidence (High/Medium/Low) with reasoning
- Be instructed to note which aspects they are MOST and LEAST qualified to assess
- Use `model: sonnet` (default) or `model: opus` (if `--deep` flag was set)

### Agent Prompt Template

For each agent, use this prompt structure:

```
You are [PERSPECTIVE NAME] on the PolyClaude Council — a panel of cognitive perspectives analyzing a question from different angles.

[FULL PERSPECTIVE DEFINITION FROM perspectives.md — Identity, Methodology, Signature Questions, Challenge Targets, Confidence Calibration]

---

QUESTION TO ANALYZE:
[User's question, verbatim]

---

INSTRUCTIONS:
1. Follow your methodology step by step
2. Apply your signature questions to this specific situation
3. Consider your challenge targets — what would the other perspectives likely argue, and where would you push back?
4. Rate your confidence (High/Medium/Low) with a specific reason
5. Note which aspects of this question you are MOST and LEAST qualified to assess
6. Use your perspective's output structure exactly

Be thorough but focused. Your analysis should be 300-600 words. Quality over quantity.
```

**Critical:** All N Agent tool calls MUST be in a single message to execute in parallel. Use `description` like "PolyClaude: The Architect" for each.

## Step 6: Synthesize

After all agents return their analyses, perform dialectical synthesis.

Read the synthesis methodology:
@${CLAUDE_PLUGIN_ROOT}/skills/council/references/synthesis.md

Follow the 7-step synthesis process, adapting thresholds to the council size:
1. Map consensus (use proportional thresholds — see synthesis.md)
2. Identify tensions (2+ perspectives disagree)
3. Resolve or frame each tension (prioritize the 2-3 most significant)
4. Detect blind spots (what nobody addressed)
5. Build confidence map (aggregate ratings) — skip for `--quick` councils of 2
6. Synthesize verdict (1-3 sentence bottom line)
7. Order next steps (3-5 actionable items)

## Step 7: Output the Council Report

Read the output format:
@${CLAUDE_PLUGIN_ROOT}/skills/council/references/output-format.md

Format your synthesis as a Council Report following the appropriate template:
- **Quick (2 perspectives):** Use the compact format — Verdict, Agreement/Disagreement, Blind Spots, Next Steps
- **Standard (3-6 perspectives):** Use the full format with all sections

Include the full individual perspective analyses in collapsible `<details>` sections at the bottom.

## Important Notes

- Do NOT editorialize between spawning agents and synthesizing. Let the perspectives speak, then synthesize.
- Do NOT show raw agent output to the user. Only show the final Council Report.
- If an agent fails or times out, proceed with remaining perspectives and note the gap: "The [X] perspective was unavailable for this analysis."
- The synthesis is YOUR job as orchestrator. Do not spawn an additional agent for synthesis.
- Be intellectually honest in synthesis — represent tensions faithfully, don't smooth them over.
- At 5-6 perspectives, prioritize the 2-3 most significant tensions rather than cataloging every disagreement.
