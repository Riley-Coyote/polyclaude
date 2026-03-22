# Question Classification & Perspective Routing

## How to Classify

Read the user's question and match it against the categories below. Use the **signal words** and **intent patterns** to determine the best fit. If the question spans multiple categories, choose the one that best captures the user's primary need. When uncertain, default to **General/Unknown**.

## Classification Table

The **User Advocate is always included**. Select 3 additional perspectives based on question type:

### Architecture / Design
**Signals:** "structure", "design", "build", "system", "architecture", "schema", "API", "database", "microservice", "monolith", "component", "module", "layer"
**Intent:** How should something be structured or organized?
**Select:** User Advocate + Architect + Skeptic + Pragmatist

### Strategy / Direction
**Signals:** "should we", "roadmap", "direction", "pivot", "bet on", "invest in", "long-term", "vision", "compete", "differentiate", "market"
**Intent:** Which path should we take? What should we commit to?
**Select:** User Advocate + Architect + Innovator + Temporal Analyst

### User Experience
**Signals:** "UX", "users", "onboarding", "adoption", "usability", "interface", "experience", "friction", "flow", "journey", "accessibility"
**Intent:** How will people experience or interact with this?
**Select:** User Advocate + Skeptic + Pragmatist + Innovator

### Risk Assessment
**Signals:** "risk", "danger", "concern", "worry", "vulnerability", "threat", "downside", "failure", "worst case", "what could go wrong"
**Intent:** What are the dangers and how do we mitigate them?
**Select:** User Advocate + Skeptic + Temporal Analyst + Architect

### Innovation / Ideation
**Signals:** "new idea", "what if", "brainstorm", "explore", "creative", "alternative", "novel", "rethink", "reimagine", "disrupt", "experiment"
**Intent:** Generate new possibilities or challenge existing approaches
**Select:** User Advocate + Innovator + Architect + Skeptic

### Planning / Execution
**Signals:** "plan", "timeline", "roadmap", "execute", "implement", "phase", "milestone", "sprint", "ship", "deliver", "prioritize", "sequence"
**Intent:** How should we order, schedule, or execute this work?
**Select:** User Advocate + Temporal Analyst + Pragmatist + Architect

### General / Unknown
**Signals:** (no clear category match)
**Intent:** Broad analysis needed
**Select:** User Advocate + Architect + Skeptic + Pragmatist

## Classification Output

After classifying, state:

```
Question Type: [category]
Council: User Advocate (permanent) + [Perspective 2] + [Perspective 3] + [Perspective 4]
```

Then proceed to spawn the 4 selected perspectives.
