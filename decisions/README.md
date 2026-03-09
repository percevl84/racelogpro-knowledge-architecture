# Strategic Decisions

## How Decisions Are Documented and Why It Matters

---

## The Problem

In AI-assisted development, decisions are made in conversation. The conversation ends, the decision evaporates. Three weeks later, you make the same decision again -- or worse, you make the opposite decision because you forgot the first one.

Over 255+ hours and 78 strategic decisions, this project developed a practice of documenting every significant decision at the moment it is made, with enough context that a future session can understand not just WHAT was decided but WHY.

---

## The Decision Format

Each decision follows a consistent structure:

```
## Decision #[N]: [Title]

Date: [When decided]
Status: APPROVED / SUPERSEDED / DEFERRED
Category: [Architecture / UX / AI / Data / Process]

### Decision
[What was decided, in 1-3 sentences]

### Rationale
[Why this option was chosen over alternatives]

### Alternatives Considered (when applicable)
[What else was on the table and why it was rejected]

### Impact
[What this affects -- which specs, code, future work]
```

---

## What Counts as a "Strategic Decision"

Not every choice needs formal documentation. The threshold:

- **Document it** if you would regret forgetting it next week
- **Document it** if it affects multiple features or files
- **Document it** if it reverses or modifies a previous decision
- **Document it** if you debated between real alternatives
- **Document it** if it establishes a pattern for future work

Quick implementation choices (which CSS class to use, which variable name) are not strategic decisions. Architectural patterns (how data flows between pages, how AI context is assembled) are.

---

## How Decisions Accumulate

The decisions in this project span the full arc of development:

| Phase | Decision Range | Themes |
|-------|---------------|--------|
| Early (Nov-Dec 2025) | #1-#11 | Scope, priorities, build order |
| Middle (Jan 2026) | #12-#52 | AI architecture, coaching methodology, knowledge base |
| Late (Feb-Mar 2026) | #53-#78 | Tool orchestration, UI overhaul, design systems, quality |

The decisions tell the story of the project's evolution. Early decisions are about "what to build." Middle decisions are about "how to make the AI smart." Late decisions are about "how to build reliably at scale."

---

## How Decisions Are Used

### During Planning
When proposing a new feature, the strategist checks the decision log: "Has this been discussed before? Is there a decision that constrains this? Did we explicitly defer this?"

### During Building
Handoff documents reference relevant decisions: "Per Decision #49, the AI adapts its voice based on location in the app, not via separate modes."

### During Disputes
When the AI suggests something that contradicts a previous approach, the decision log provides the authoritative answer. "Decision #39 established ASCII-only formatting for all documents -- smart quotes are not used."

### During Onboarding
A new AI session can read the decision log to understand the project's design philosophy without needing to infer it from code. Decisions are explicit intent, not implicit patterns.

---

## Example Decisions

See [example-decisions.md](example-decisions.md) for 8 representative decisions that illustrate the pattern.

---

*78 strategic decisions documented over 255+ hours of development, November 2025 -- March 2026.*
