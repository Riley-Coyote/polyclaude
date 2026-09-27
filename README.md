# PolyClaude

**A council of distinct minds for Claude Code.**

PolyClaude convenes a council on your question, plan, or idea. The members are six built-in cognitive lenses, or experts you name. Each one analyzes the question alone. Then they read each other's arguments and answer them: they challenge, concede, hold their ground, or change their minds. What survives is synthesized into a decision document.

The output isn't a list of opinions. It's a verdict, the consensus that held under challenge, what moved in cross-examination, the tensions that are real trade-offs, the blind spots nobody caught, and a confidence map.

---

## Installation

```bash
# Add the marketplace (one-time)
claude plugin marketplace add Riley-Coyote/polyclaude

# Install the plugin
claude plugin install polyclaude@polyclaude
```

Restart Claude Code after installing.

### Updating

```bash
claude plugin marketplace update polyclaude
claude plugin update polyclaude@polyclaude
```

Then restart Claude Code.

> **Troubleshooting:** If adding the marketplace fails with an SSH error, your machine is trying to use SSH for GitHub, but the keys aren't set up. Run this to force HTTPS:
> ```bash
> git config --global url."https://github.com/".insteadOf "git@github.com:"
> ```
> Then add the marketplace again.

---

## Usage

There are two ways in, and both run the same council:

```
/polyclaude Should we rewrite the auth system or patch it incrementally?
/polyclaude:council Should we rewrite the auth system or patch it incrementally?
```

Or just ask in conversation: "run this through the council", "red team this plan", "get a panel of experts on this".

### Council size

| Flag | Members | Rounds |
|------|---------|--------|
| `--quick` | 2 | Independent only: a fast sanity check |
| *(default)* | 4 | Independent, then cross-examination |
| `--full` | 6 | Independent, then cross-examination |
| `--council N` | 2–6 | Independent, then cross-examination |

`--no-debate` skips cross-examination at any size.

### Expert panels

Name the council you want. PolyClaude builds a full member for each role: its own method, the questions it asks first, who on the panel it will push back on, and where it should defer.

```
/polyclaude --panel "master typographer; veteran interface engineer; accessibility specialist" Which type system should the landing page use?
/polyclaude:council Get world-class memory-systems researchers on whether we should drop the vector store.
```

Naming experts in plain words works the same as `--panel`. Someone always speaks for the people the decision lands on, unless you pass `--exclude advocate`.

### Quality

| Flag | What it does |
|------|-------------|
| *(default)* | Sonnet members: fast and cost-effective |
| `--deep` | Opus members: maximum depth |

### Built-in lens control

```
/polyclaude --include temporal,innovator Should we refactor the API?
/polyclaude --exclude pragmatist What if we open-sourced everything?
```

Lens names: `architect`, `skeptic`, `pragmatist`, `innovator`, `advocate`, `temporal`. An excluded lens's seat passes to the next lens in line, so the council keeps its size.

### Cost and time

Measured at API prices with an Opus main session and Sonnet members. The figures vary with the length of the brief and how much the members read.

| Council | Whole council | Typical time |
|---------|---------------|--------------|
| 2 members, one round (`--quick`) | ~$0.70–1.10 | ~3 min |
| 4 members, one round (`--no-debate`) | ~$1.00–1.50 | ~5 min |
| 3–4 members with cross-examination (default) | ~$2.00–3.00 | ~8–10 min |
| 6 members with cross-examination (`--full`) | ~$3.50–5.00 | ~12–15 min |

Most of the cost (about 80%) is the main session running the council. `--deep` members cost about 2.5× Sonnet members, which adds roughly a third to the total. With a Sonnet main session, the whole council costs roughly half. On a Claude subscription, all of this comes out of your usage limits rather than dollars.

---

## How It Works

```
/polyclaude "Should we use a monorepo or polyrepo?"
                          │
                   WRITE THE BRIEF
          question · context · materials · scope
                          │
                  COMPOSE THE COUNCIL
     classify → built-in lenses,  or  build an expert panel
                          │
      ┌─────────────┬─────┴───────┬─────────────┐
      ▼             ▼             ▼             ▼
  USER ADVOCATE  ARCHITECT     SKEPTIC     PRAGMATIST     ROUND 1
      │             │             │             │         independent, in parallel
      └─────────────┴─ read each other's work ──┘
      ▼             ▼             ▼             ▼
     challenge · concede · hold · change                  ROUND 2
      │             │             │             │         cross-examination, in parallel
      └─────────────┴──────┬──────┴─────────────┘
                           ▼
                 DIALECTICAL SYNTHESIS
     what held · what moved · what persisted · blind spots
                           │
                     COUNCIL REPORT
```

1. **Brief.** Members are subagents and can't see your conversation. So the orchestrator writes them one shared brief: the question, the context, materials they may read, and the scope. Every member gets the same brief, so their differences come from their lens, not from what they were told.
2. **Compose.** The question is classified and the most relevant lenses take the seats. If you named experts, a full card is written for each one instead.
3. **Round 1.** Every member analyzes independently and in parallel, so nobody anchors on anybody else.
4. **Round 2: cross-examination.** Every member reads the load-bearing claims the others wrote down, in their own words. Each one:
   - names the strongest point against them,
   - challenges the claims they most disagree with, naming the test that would settle each,
   - concedes what landed,
   - declares their position HELD, SHARPENED, or CHANGED.
5. **Synthesis.** Finds the consensus that survived challenge, who moved and why, the tensions that persisted (the real trade-offs), and the blind spots nobody addressed.
6. **Report.** A Council Report you can act on.

---

## The Council

Six built-in lenses. The User Advocate is seated by default.

### The User Advocate *(seated by default)*
> "How does this feel to encounter for the first time?"

Thinks from the perspective of whoever will actually use, encounter, or be affected by this decision. Cares about first impressions, learning curves, emotional responses, and accessibility. The voice of the person who wasn't in the room.

### The Architect
> "What are the load-bearing assumptions?"

Systems thinker. Sees structure, connections, dependencies, and feedback loops. Maps components and relationships, assesses scalability, and finds structural weaknesses. Thinks in diagrams.

### The Skeptic
> "What are we not seeing?"

Forensic truth-seeker. Finds cracks before they become failures. Audits assumptions, identifies failure modes, and surfaces edge cases. Not negative, but rigorously honest: the one who saves the team from shipping a disaster.

### The Pragmatist
> "What's the simplest thing that works?"

Practitioner. Cares about what actually works under real constraints. Assesses effort against value, finds what can be deferred, and estimates real-world complexity. Respects elegance, but not at the expense of shipping.

### The Innovator
> "What would the opposite approach look like?"

Divergent thinker. Generates genuine alternatives the room hasn't considered. Inverts problems, finds cross-domain analogies, and questions hidden constraints. Expands the solution space before it narrows.

### The Temporal Analyst
> "What does this look like in 6 months?"

Time-aware strategist. Analyzes the movie, not the snapshot. Maps timelines, identifies critical-path dependencies, finds second-order effects, and assesses reversibility. The one who asks "and then what?"

---

## Adaptive Lens Selection

Each question type has a relevance order, and the most relevant lenses take the seats first:

| Question Type | Relevance Order (after User Advocate) |
|---|---|
| **Architecture / Design** | Architect > Skeptic > Pragmatist > Temporal > Innovator |
| **Strategy / Direction** | Architect > Innovator > Temporal > Skeptic > Pragmatist |
| **User Experience** | Skeptic > Pragmatist > Innovator > Temporal > Architect |
| **Risk Assessment** | Skeptic > Temporal > Architect > Pragmatist > Innovator |
| **Innovation / Ideation** | Innovator > Architect > Skeptic > Temporal > Pragmatist |
| **Planning / Execution** | Temporal > Pragmatist > Architect > Skeptic > Innovator |
| **General** | Architect > Skeptic > Pragmatist > Innovator > Temporal |

With `--quick` (2) you get the User Advocate plus the top-ranked lens. The default (4) seats the top 3, and `--full` (6) seats all of them.

---

## What You Get: The Council Report

**Verdict:** a 1–3 sentence bottom line. What to do, not what to think about.

**Consensus Points:** where a strong majority agreed. Each one is marked *held under challenge* or *unchallenged*, because agreement nobody tested is weaker than agreement that survived an attack.

**What Moved in Cross-Examination:** who sharpened or changed their position and what moved them, which challenges landed, and who held firm.

**Key Tensions:** structured disagreements between competing values, with the actual exchange:

> **Tension: Speed vs. Correctness**
> - **The Pragmatist** argues for shipping the incremental patch now, because the rewrite blocks the team for 6 weeks.
> - **The Architect** counters that the patch adds structural debt that will cost 3x more to fix later.
> - **In cross-examination:** the Pragmatist conceded the debt is real, but held that a dated rewrite plan contains it. The Architect sharpened to "patch only with a rewrite deadline".
> - **Resolution:** Ship the patch with a hard deadline for the rewrite. Accept the debt consciously, with a plan to retire it.

**Blind Spots:** what NO member addressed. Gaps that could change the recommendation.

**Confidence Map:** where the analysis is certain and where it isn't, after cross-examination.

**Recommended Next Steps:** ordered, actionable items. Cheap tests that would settle the key tension come early.

**Individual Perspectives:** each member's full round-1 analysis and cross-examination, in collapsible sections.

Two-member councils use a compact format: Verdict, Agreement/Disagreement, Blind Spots, Next Steps.

---

## What Makes This Different

- **Members actually argue.** In round 2, every member reads the others and has to answer them. The report tells you which conclusions survived that and which nobody tested.
- **You get the council you ask for.** Name the experts and they're built with real methods, not generic lenses wearing name tags.
- **Grounded, and read-only by construction.** Members share a brief and can read, search, and browse what it points to. They run as a dedicated agent with no write, edit, or shell tools, so the council can't change your project. It only advises.
- **Independent first.** Round 1 runs in parallel and in isolation, so nobody anchors on anybody else before the debate.
- **Dialectical synthesis.** It finds tensions between values, proposes resolutions, and names the test that would settle a trade-off.
- **Blind spot detection.** It explicitly surfaces what nobody addressed.
- **Decision-ready output.** You get a document to act on, not a discussion to interpret.

---

## Plugin Structure

```
polyclaude/
├── .claude-plugin/
│   ├── plugin.json                  # Plugin manifest
│   └── marketplace.json             # Marketplace manifest for distribution
├── agents/
│   └── council-member.md            # Read-only council member (Read, Glob, Grep, WebFetch, WebSearch)
├── commands/
│   └── polyclaude.md                # /polyclaude: a doorway into the council skill
└── skills/
    └── council/
        ├── SKILL.md                 # The full procedure: brief → compose → rounds → synthesis → report
        └── references/
            ├── perspectives.md      # The six built-in lens cards
            ├── classification.md    # Question types, relevance order, seat selection
            ├── expert-panels.md     # Building members for experts you name
            ├── cross-examination.md # Round 2: prompt and rules
            ├── synthesis.md         # Dialectical synthesis, weighted by what survived round 2
            └── output-format.md     # Council Report templates (compact + standard)
```

---

## Roadmap

- **v0.1:** Core plugin: 4 perspectives, adaptive selection, dialectical synthesis
- **v0.2:** Configurable council: `--quick`/`--full`, `--include`/`--exclude`, proportional synthesis, cost estimation
- **v0.3:** Expert panels, a cross-examination round, a shared brief, read-only members, and one procedure behind both `/polyclaude` and `/polyclaude:council` *(current)*
- **Next** (both recommended by PolyClaude's own councils while v0.3 was being tested):
  - **Saved councils:** write each run (the brief, both rounds, the report) to a git-ignored `.polyclaude/councils/` folder, so decisions can be revisited, shared, and checked against what actually happened.
  - **Debate only where it's needed:** run cross-examination only when round-1 positions actually diverge.
- **Later:** Saved panels (reusable expert cards as drop-in `.md` files), and a research round where members gather evidence before they judge.

---

## Requirements

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI
- Works with any Claude model (Opus recommended for orchestration)

---

## License

MIT
