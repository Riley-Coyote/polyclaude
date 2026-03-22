# Question Classification & Perspective Routing

## How to Classify

Read the user's question and match it against the categories below. Use the **signal words** and **intent patterns** to determine the best fit. If the question spans multiple categories, choose the one that best captures the user's primary need. When uncertain, default to **General/Unknown**.

## Council Size

The number of perspectives depends on the user's flags:

| Flag | Council Size | Selection Method |
|---|---|---|
| `--quick` | 2 | User Advocate + 1 most relevant |
| *(default)* | 4 | User Advocate + 3 adaptive |
| `--full` | 6 | All perspectives |
| `--council N` | N (2-6) | User Advocate + (N-1) adaptive |

## Classification Table

### Architecture / Design
**Signals:** "structure", "design", "build", "system", "architecture", "schema", "API", "database", "microservice", "monolith", "component", "module", "layer"
**Intent:** How should something be structured or organized?
**Relevance order:** Architect > Skeptic > Pragmatist > Temporal Analyst > Innovator

### Strategy / Direction
**Signals:** "should we", "roadmap", "direction", "pivot", "bet on", "invest in", "long-term", "vision", "compete", "differentiate", "market"
**Intent:** Which path should we take? What should we commit to?
**Relevance order:** Architect > Innovator > Temporal Analyst > Skeptic > Pragmatist

### User Experience
**Signals:** "UX", "users", "onboarding", "adoption", "usability", "interface", "experience", "friction", "flow", "journey", "accessibility"
**Intent:** How will people experience or interact with this?
**Relevance order:** Skeptic > Pragmatist > Innovator > Temporal Analyst > Architect

### Risk Assessment
**Signals:** "risk", "danger", "concern", "worry", "vulnerability", "threat", "downside", "failure", "worst case", "what could go wrong"
**Intent:** What are the dangers and how do we mitigate them?
**Relevance order:** Skeptic > Temporal Analyst > Architect > Pragmatist > Innovator

### Innovation / Ideation
**Signals:** "new idea", "what if", "brainstorm", "explore", "creative", "alternative", "novel", "rethink", "reimagine", "disrupt", "experiment"
**Intent:** Generate new possibilities or challenge existing approaches
**Relevance order:** Innovator > Architect > Skeptic > Temporal Analyst > Pragmatist

### Planning / Execution
**Signals:** "plan", "timeline", "roadmap", "execute", "implement", "phase", "milestone", "sprint", "ship", "deliver", "prioritize", "sequence"
**Intent:** How should we order, schedule, or execute this work?
**Relevance order:** Temporal Analyst > Pragmatist > Architect > Skeptic > Innovator

### General / Unknown
**Signals:** (no clear category match)
**Intent:** Broad analysis needed
**Relevance order:** Architect > Skeptic > Pragmatist > Innovator > Temporal Analyst

## Perspective Selection Algorithm

1. Classify the question to determine the relevance order
2. Start with User Advocate (always included unless `--exclude advocate`)
3. Fill remaining slots from the relevance order, top to bottom, until council size is reached
4. Apply `--include` overrides: add named perspectives if not already selected (may push council size up to 6 max)
5. Apply `--exclude` overrides: remove named perspectives from the council
6. Ensure final council has at least 2 perspectives

### Examples

**`/polyclaude Should we use Redis or Postgres for sessions?`**
→ Architecture. Default (4). Council: User Advocate + Architect + Skeptic + Pragmatist

**`/polyclaude --quick Should we use Redis or Postgres?`**
→ Architecture. Quick (2). Council: User Advocate + Architect

**`/polyclaude --full Should we use Redis or Postgres?`**
→ Architecture. Full (6). Council: User Advocate + Architect + Skeptic + Pragmatist + Temporal Analyst + Innovator

**`/polyclaude --include temporal Should we use Redis or Postgres?`**
→ Architecture. Default (4) + forced include. Council: User Advocate + Architect + Skeptic + Pragmatist + Temporal Analyst (5 total)

**`/polyclaude --exclude architect What's our mobile strategy?`**
→ Strategy. Default (4) minus Architect. Council: User Advocate + Innovator + Temporal Analyst + Skeptic (4 total — next in relevance order fills the gap)

## Classification Output

After resolving, state:

```
Question Type: [category]
Council ([N] perspectives): [Perspective 1] + [Perspective 2] + ...
```

Then proceed to spawn the selected perspectives.
