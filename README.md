# PolyClaude

**Multi-perspective council analysis for Claude Code.**

PolyClaude spawns parallel cognitive perspectives to analyze your questions, plans, and ideas from multiple angles — then synthesizes the results through structured dialectical analysis. Instead of one viewpoint, you get a council of distinct thinkers who agree, disagree, and challenge each other.

The output isn't a list of opinions. It's a decision document: consensus points, structured tensions, blind spots nobody caught, a confidence map, and a clear verdict.

---

## Installation

```bash
# Add the marketplace (one-time)
claude plugin marketplace add Riley-Coyote/polyclaude

# Install the plugin
claude plugin install polyclaude@polyclaude
```

Restart Claude Code after installing. The `/polyclaude` command will be available in all sessions.

> **Troubleshooting:** If the marketplace add fails with an SSH error, your machine is trying to use SSH for GitHub but the keys aren't set up. Run this to force HTTPS:
> ```bash
> git config --global url."https://github.com/".insteadOf "git@github.com:"
> ```
> Then retry the marketplace add command.

---

## Usage

```
/polyclaude Should we rewrite the auth system or patch it incrementally?
```

```
/polyclaude --deep What's the right architecture for our real-time notification system?
```

| Flag | What it does |
|------|-------------|
| *(none)* | Sonnet agents + Opus synthesis — fast, cost-effective (~$0.30) |
| `--deep` | All Opus — maximum analytical depth on every perspective (~$1.50) |

---

## How It Works

```
/polyclaude "Should we use a monorepo or polyrepo?"
                         |
                    CLASSIFY QUESTION
                    Type: Architecture
                         |
         ┌───────────────┼───────────────┐───────────────┐
         v               v               v               v
    ARCHITECT        SKEPTIC        PRAGMATIST      USER ADVOCATE
    (parallel)       (parallel)     (parallel)      (always present)
         |               |               |               |
         └───────────────┴───────────────┴───────────────┘
                         |
                  DIALECTICAL SYNTHESIS
                  1. Map consensus
                  2. Identify tensions
                  3. Resolve or frame trade-offs
                  4. Detect blind spots
                  5. Build confidence map
                  6. Synthesize verdict
                         |
                   COUNCIL REPORT
```

1. **Classify** — PolyClaude reads your question and determines the type (architecture, strategy, UX, risk, innovation, planning)
2. **Select** — Picks the 4 most relevant perspectives. The User Advocate is always included; 3 others are chosen adaptively based on question type
3. **Analyze** — Spawns 4 agents in parallel, each analyzing from their distinct cognitive lens
4. **Synthesize** — Performs dialectical synthesis: not just summarizing what each said, but finding agreements, tensions, and blind spots
5. **Report** — Outputs a structured Council Report you can act on immediately

---

## The Council

Six cognitive perspectives are available. Four are selected per question.

### The User Advocate *(always included)*
> "How does this feel to encounter for the first time?"

Thinks from the perspective of whoever will actually use, encounter, or be affected by this decision. Cares about first impressions, learning curves, emotional responses, and accessibility. The voice of the person who wasn't in the room.

### The Architect
> "What are the load-bearing assumptions?"

Systems thinker. Sees structure, connections, dependencies, and feedback loops. Maps components and relationships, assesses scalability, finds structural weaknesses. Thinks in diagrams.

### The Skeptic
> "What are we not seeing?"

Forensic truth-seeker. Finds cracks before they become failures. Audits assumptions, identifies failure modes, surfaces edge cases. Not negative — rigorously honest. The one who saves the team from shipping a disaster.

### The Pragmatist
> "What's the simplest thing that works?"

Practitioner. Cares about what actually works with real constraints. Assesses effort-to-value ratios, finds what can be deferred, estimates real-world complexity. Respects elegance but not at the expense of shipping.

### The Innovator
> "What would the opposite approach look like?"

Divergent thinker. Generates genuine alternatives the room hasn't considered. Inverts problems, finds cross-domain analogies, questions hidden constraints. Expands the solution space before it narrows.

### The Temporal Analyst
> "What does this look like in 6 months?"

Time-aware strategist. Analyzes the movie, not the snapshot. Maps timelines, identifies critical path dependencies, finds second-order effects, assesses reversibility. The one who asks "and then what?"

---

## Adaptive Perspective Selection

The council composition adapts to your question:

| Question Type | Perspectives Selected |
|---|---|
| **Architecture / Design** | User Advocate + Architect + Skeptic + Pragmatist |
| **Strategy / Direction** | User Advocate + Architect + Innovator + Temporal Analyst |
| **User Experience** | User Advocate + Skeptic + Pragmatist + Innovator |
| **Risk Assessment** | User Advocate + Skeptic + Temporal Analyst + Architect |
| **Innovation / Ideation** | User Advocate + Innovator + Architect + Skeptic |
| **Planning / Execution** | User Advocate + Temporal Analyst + Pragmatist + Architect |
| **General** | User Advocate + Architect + Skeptic + Pragmatist |

---

## What You Get: The Council Report

Every `/polyclaude` invocation produces a structured Council Report:

### Verdict
A 1-3 sentence bottom-line recommendation. What to do, not what to think about.

### Consensus Points
Where 3+ perspectives agree. These are your highest-confidence findings.

### Key Tensions
Structured disagreements — not "A said X, B said Y" but genuine tensions between competing values:

> **Tension: Speed vs. Correctness**
> - The Pragmatist argues for shipping the incremental patch now because the rewrite blocks the team for 6 weeks
> - The Architect counters that the patch adds to structural debt that will cost 3x more to fix later
> - **Resolution:** Ship the patch with a hard deadline for the rewrite. Accept the debt consciously with a plan to retire it.

### Blind Spots
What NO perspective addressed — gaps in the analysis that could change the recommendation.

### Confidence Map
| Aspect | Confidence | Signal |
|--------|-----------|--------|
| Technical feasibility | High | All perspectives agree |
| Timeline estimate | Low | Divergent views, needs research |
| User adoption | Medium | User Advocate confident, Skeptic cautious |

### Recommended Next Steps
Ordered, actionable items — start with the most urgent and highest-confidence actions.

### Individual Perspectives
Full analysis from each council member, available in collapsible sections for reference.

---

## What Makes This Different

Most "multi-perspective" tools just run the same prompt with different system messages. PolyClaude is structurally different:

- **Adaptive council** — Question classification selects the most relevant perspectives, not a fixed set
- **Dialectical synthesis** — Finds tensions between values, proposes resolutions, doesn't just summarize
- **Blind spot detection** — Explicitly surfaces what nobody addressed
- **Confidence mapping** — Shows where the analysis is certain vs. uncertain
- **Cross-perspective challenges** — Each perspective is prompted to challenge the likely arguments of other perspectives
- **Decision-ready output** — You get a document you can act on, not a discussion to interpret

---

## Plugin Structure

```
polyclaude/
├── .claude-plugin/
│   ├── plugin.json              # Plugin manifest
│   └── marketplace.json         # Marketplace manifest for distribution
├── commands/
│   └── polyclaude.md            # /polyclaude slash command
└── skills/
    └── council/
        ├── SKILL.md             # Auto-triggers on "multiple perspectives" etc.
        └── references/
            ├── perspectives.md  # 6 perspective definitions
            ├── classification.md # Question type routing
            ├── synthesis.md     # Dialectical synthesis methodology
            └── output-format.md # Council Report template
```

---

## Requirements

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI
- Works with any Claude model (Opus recommended for orchestration)

---

## License

MIT
