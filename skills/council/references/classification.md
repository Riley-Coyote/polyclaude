# Question Classification & Lens Selection

Use this when the council is built from the six built-in lenses. For expert panels, see `expert-panels.md`.

## How to Classify

Read the user's question and match it against the categories below, using the **signal words** and **intent patterns**. If the question spans several categories, choose the one that best captures the user's primary need. When uncertain, default to **General / Unknown**.

## Council Size

| Flag | Members | Seated as |
|---|---|---|
| `--quick` | 2 | User Advocate + the top lens (independent round only) |
| *(default)* | 4 | User Advocate + the top 3 |
| `--full` | 6 | All six |
| `--council N` | N (2–6) | User Advocate + the top N−1 |

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

## Selection Algorithm

1. Classify the question to get its relevance order.
2. Take `--exclude`d lenses out of consideration first, including `advocate` if it's named.
3. Seat the User Advocate, unless it was excluded.
4. Fill the remaining seats from the relevance order, top to bottom, until the council size is reached. Exclusions are already gone, so an excluded lens's seat passes to the next lens in line.
5. Seat any `--include`d lenses that aren't already present. This can grow the council, up to 6.
6. A lens that is both included and excluded stays out. If exclusions leave fewer lenses than seats, the council is simply smaller, but never fewer than 2.

### Examples

**`/polyclaude Should we use Redis or Postgres for sessions?`**
→ Architecture. Default (4). Council: User Advocate + Architect + Skeptic + Pragmatist

**`/polyclaude --quick Should we use Redis or Postgres?`**
→ Architecture. Quick (2). Council: User Advocate + Architect

**`/polyclaude --full Should we use Redis or Postgres?`**
→ Architecture. Full (6). Council: User Advocate + Architect + Skeptic + Pragmatist + Temporal Analyst + Innovator

**`/polyclaude --include temporal Should we use Redis or Postgres?`**
→ Architecture. Default (4) + forced include. Council: User Advocate + Architect + Skeptic + Pragmatist + Temporal Analyst (5)

**`/polyclaude --exclude architect What's our mobile strategy?`**
→ Strategy. Default (4), Architect excluded. Council: User Advocate + Innovator + Temporal Analyst + Skeptic (4; the Skeptic takes the open seat)

**`/polyclaude --full --exclude pragmatist Should we pivot to enterprise?`**
→ Strategy. Full (6), Pragmatist excluded. Council: User Advocate + Architect + Innovator + Temporal Analyst + Skeptic (5; only five lenses remain)
