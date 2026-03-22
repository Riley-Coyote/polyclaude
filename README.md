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

### Basic

```
/polyclaude Should we rewrite the auth system or patch it incrementally?
```

### Council Size

| Flag | Perspectives | Use Case |
|------|-------------|----------|
| `--quick` | 2 | Fast sanity check |
| *(default)* | 4 | Balanced analysis |
| `--full` | 6 | Comprehensive deep dive |
| `--council N` | 2-6 | Exact count (power users) |

```
/polyclaude --quick Is this API design reasonable?
/polyclaude --full What's our mobile strategy for next year?
```

### Quality

| Flag | What it does |
|------|-------------|
| *(default)* | Sonnet agents — fast, cost-effective |
| `--deep` | Opus agents — maximum analytical depth |

```
/polyclaude --deep What architecture should we use for real-time sync?
/polyclaude --full --deep Should we pivot to enterprise?
```

### Perspective Control

Override which perspectives participate:

```
/polyclaude --include temporal,innovator Should we refactor the API?
/polyclaude --exclude pragmatist What if we open-sourced everything?
```

Perspective names: `architect`, `skeptic`, `pragmatist`, `innovator`, `advocate`, `temporal`

### Estimated Costs

| Mode | Perspectives | Sonnet (default) | Opus (--deep) |
|------|-------------|-----------------|---------------|
| `--quick` | 2 | ~$0.15 | ~$0.75 |
| *(default)* | 4 | ~$0.30 | ~$1.50 |
| `--full` | 6 | ~$0.45 | ~$2.25 |

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
2. **Select** — Picks the most relevant perspectives based on council size. The User Advocate is always included; others are chosen adaptively by question type
3. **Analyze** — Spawns N agents in parallel, each analyzing from their distinct cognitive lens
4. **Synthesize** — Performs dialectical synthesis: not just summarizing what each said, but finding agreements, tensions, and blind spots
5. **Report** — Outputs a structured Council Report you can act on immediately

---

## The Council

Six cognitive perspectives are available. The User Advocate is always included by default.

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

The council composition adapts to your question. Each question type has a relevance order — the most relevant perspectives are selected first:

| Question Type | Relevance Order (after User Advocate) |
|---|---|
| **Architecture / Design** | Architect > Skeptic > Pragmatist > Temporal > Innovator |
| **Strategy / Direction** | Architect > Innovator > Temporal > Skeptic > Pragmatist |
| **User Experience** | Skeptic > Pragmatist > Innovator > Temporal > Architect |
| **Risk Assessment** | Skeptic > Temporal > Architect > Pragmatist > Innovator |
| **Innovation / Ideation** | Innovator > Architect > Skeptic > Temporal > Pragmatist |
| **Planning / Execution** | Temporal > Pragmatist > Architect > Skeptic > Innovator |
| **General** | Architect > Skeptic > Pragmatist > Innovator > Temporal |

With `--quick` (2), you get User Advocate + the top-ranked perspective. With default (4), the top 3. With `--full` (6), all of them.

---

## What You Get: The Council Report

### Standard Report (3-6 perspectives)

**Verdict** — 1-3 sentence bottom-line recommendation. What to do, not what to think about.

**Consensus Points** — Where a strong majority of perspectives agree. Your highest-confidence findings.

**Key Tensions** — Structured disagreements between competing values:

> **Tension: Speed vs. Correctness**
> - The Pragmatist argues for shipping the incremental patch now because the rewrite blocks the team for 6 weeks
> - The Architect counters that the patch adds to structural debt that will cost 3x more to fix later
> - **Resolution:** Ship the patch with a hard deadline for the rewrite. Accept the debt consciously with a plan to retire it.

**Blind Spots** — What NO perspective addressed. Gaps that could change the recommendation.

**Confidence Map** — Where the analysis is certain vs. uncertain:
| Aspect | Confidence | Signal |
|--------|-----------|--------|
| Technical feasibility | High | All perspectives agree |
| Timeline estimate | Low | Divergent views, needs research |

**Recommended Next Steps** — Ordered, actionable items.

**Individual Perspectives** — Full analyses in collapsible sections for reference.

### Quick Report (2 perspectives)

A compact format: Verdict, Agreement/Disagreement, Blind Spots, Next Steps. No Confidence Map. Fast and focused.

---

## What Makes This Different

Most "multi-perspective" tools just run the same prompt with different system messages. PolyClaude is structurally different:

- **Adaptive council** — Question classification selects the most relevant perspectives, not a fixed set
- **Configurable depth** — From a 2-perspective sanity check to a 6-perspective deep dive
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
            ├── classification.md # Question type routing with relevance ordering
            ├── synthesis.md     # Dialectical synthesis methodology
            └── output-format.md # Council Report templates (quick + standard)
```

---

## Roadmap

- **v0.1** — Core plugin: 4 perspectives, adaptive selection, dialectical synthesis
- **v0.2** — Configurable council: `--quick`/`--full`, `--include`/`--exclude`, proportional synthesis, cost estimation *(current)*
- **v0.3** — Custom perspectives: drop-in `.md` files for domain-specific perspectives
- **v1.0** — Adversarial rounds: `--debate` flag for structured perspective-vs-perspective rebuttals

---

## Requirements

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI
- Works with any Claude model (Opus recommended for orchestration)

---

## License

MIT
