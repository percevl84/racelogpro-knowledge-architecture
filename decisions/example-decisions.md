# Example Strategic Decisions

## 8 Decisions That Illustrate the Pattern

These are real decisions from the project, sanitized to remove product-specific IP while preserving the decision-making structure. They span the full arc of development from early architectural choices to late-stage process improvements.

---

## Decision #3: Knowledge Base Integration Approach

**Date:** December 30, 2025
**Status:** APPROVED
**Category:** AI Architecture

### Decision
Embed curated knowledge directly into AI prompt templates rather than building a retrieval-augmented generation (RAG) system.

### Alternatives Considered

**Option A: RAG-style retrieval (NOT CHOSEN)**
Store 1,300+ concepts in a searchable database. At runtime, find relevant concepts via embedding similarity and inject matched results into prompts.
- Pro: Scalable, dynamic, adapts to new content automatically
- Con: Complex infrastructure, requires embedding/search system, adds latency, less control over what gets injected

**Option B: Pre-embedded in prompts (CHOSEN)**
Curate key concepts from the knowledge base directly into prompt templates. Different prompts for different contexts, each containing the relevant knowledge for that context.
- Pro: Simple, fast, precise control over what the AI knows in each context
- Con: Manual curation, harder to scale to thousands of concepts

### Rationale
At the current scale (solo developer, personal use), simplicity and control outweigh scalability. The knowledge base has 1,379 concepts across 12 books, but any given prompt context only needs 20-50 relevant concepts. Pre-embedding gives precise control over which concepts appear where, without the latency and complexity of runtime retrieval.

If the user base grows to the point where dynamic retrieval is needed, the curated prompts become the gold standard for evaluating retrieval quality.

---

## Decision #39: ASCII-Only Formatting Standard

**Date:** January 6, 2026
**Status:** APPROVED
**Category:** Process / Infrastructure

### Decision
All project documents use ASCII-only characters. Smart quotes become straight quotes. Em dashes become double hyphens. Arrows become `->`. No box-drawing characters, no Unicode symbols.

### Rationale
Multiple AI tools read and write these documents. Each tool handles Unicode differently. Smart quotes introduced by one tool were corrupted by another, creating broken formatting that required manual cleanup. The ASCII-only rule eliminates an entire class of cross-tool compatibility issues.

### Substitution Table
```
Smart quotes  ->  Straight quotes (" and ')
Em dash       ->  --
Arrows        ->  -> and <-
Ellipsis      ->  ...
Box drawing   ->  |-- and +-- and `--
```

### Impact
Every document in the project (140+) follows this rule. Every AI tool's instructions include this rule. The formatting corruption incidents dropped to zero after adoption.

---

## Decision #49: Adaptive AI Presence (Not Separate Modes)

**Date:** January 11, 2026
**Status:** APPROVED
**Category:** AI Architecture

### Decision
The AI coaching assistant adapts its voice and depth based on where the user is in the application, rather than having separate "modes" the user selects.

| Location | AI Behavior |
|----------|------------|
| Home page | Brief, motivational, action-oriented |
| Strategy/planning views | Analytical advisor, longer-form reasoning |
| Chat interface | Conversational, responsive to user's tone |
| Pre-session briefing | Tactical, focused on immediate preparation |

### Alternatives Considered
A "copilot mode" toggle that switches the AI between coaching, analysis, and conversation modes. Rejected because it adds cognitive overhead for the user and creates an artificial separation between capabilities that should feel seamless.

### Rationale
The AI's insights should be woven into every part of the application. The user should not have to choose between "coaching mode" and "analysis mode" -- the AI should recognize the context and adjust automatically. This mirrors how a real human coach would behave: strategic in planning meetings, tactical in pre-race briefings, conversational over coffee.

---

## Decision #53: Three-Tool Workflow

**Date:** January 26, 2026
**Status:** APPROVED
**Category:** Process / Architecture

### Decision
Formalize three distinct AI tool roles: Strategist (planning, specs, handoffs), Implementer (full builds with codebase context), and Surgeon (targeted fixes, large document edits).

### What Triggered This
The project was using tools interchangeably. The strategist was writing code. The implementer was making architectural decisions. The surgeon was taking on full page builds. Each individual session seemed fine, but the accumulated drift -- inconsistent patterns, undocumented decisions, code that did not match specs -- was compounding.

### Rules Established
- Full page builds and 4+ file changes -> Implementer (needs codebase indexing)
- 1-3 file targeted fixes -> Surgeon (fast, focused)
- 50KB+ document edits -> Surgeon (handles large files reliably)
- Planning, decisions, specs, handoffs -> Strategist
- No exceptions without explicit discussion

### Impact
This decision was the foundation for the handoff protocol, the smoke test framework, and the entire document-mediated communication system between tools.

---

## Decision #57: Preview Before Build

**Date:** February 7, 2026
**Status:** APPROVED
**Category:** UX / Process

### Decision
Every UI component gets an interactive HTML preview before any production code is written. The preview is iterated with the human until approved, then becomes the visual reference in the handoff document.

### Rationale
Changing an HTML mockup takes seconds. Changing a React component wired to a backend takes hours. The preview step catches visual mismatches, layout problems, and "that's not what I meant" moments before any production code exists.

### Process
1. Strategist generates HTML preview from the spec
2. Human reviews: "move this," "wrong spacing," "different feel"
3. Iterate until approved (usually 2-4 rounds, 15-30 minutes)
4. Approved preview becomes part of the handoff document
5. Implementer builds to match the approved preview

### When to Skip
- Backend-only work (no visual component)
- Bug fixes (correct behavior already defined)
- Schema changes
- Refactors (same behavior, different implementation)

### Impact
Four full pages built using this pattern. Zero "that's not what I wanted" incidents after adoption. Previously, approximately one visual mismatch per build required rework.

---

## Decision #62: Intelligence Before Aesthetics

**Date:** February 7, 2026
**Status:** APPROVED
**Category:** UX Architecture / Prioritization

### Decision
The visual polish pass (frameless window, refined navigation, dark theme polish) happens AFTER the intelligence roadmap is complete. Coaching intelligence and data quality come first; aesthetics second.

### Rationale
A beautiful app that gives mediocre coaching advice fails at its core purpose. A visually rough app that provides genuinely helpful, context-aware coaching succeeds at its core purpose and can be polished later.

The frameless window and navigation refinements do not affect data flow, coaching quality, or user outcomes. The intelligence improvements (contextual accuracy, plan generation, pattern detection) do.

### Impact
This decision shaped the Phase 10 build order: all intelligence and data wiring was completed before any visual polish. The visual shells were built with the final design but wired to live data and AI before any cosmetic refinements.

---

## Decision #65: Home Page as Coaching Hub (Not Dashboard)

**Date:** February 10, 2026
**Status:** APPROVED
**Category:** UX Architecture

### Decision
Returning users see a coaching-oriented home page -- a contextual greeting, today's focus, and a preview of their progress -- not a traditional dashboard with charts and metrics.

### Alternatives Considered
A data-heavy dashboard (charts, tables, statistics) as the default landing page. Rejected because it prioritizes information consumption over action-taking. The user opens the app to get better, not to look at charts.

### Rationale
Habit science research (from the project's knowledge base of 1,379 concepts) shows that immediate recognition and visible progress on every return visit drive sustained engagement. A single clear action item is more effective than a wall of data, particularly for users with attention challenges.

The detailed data views exist -- they are one click away. But the first thing the user sees is their coach acknowledging them and pointing them toward today's work.

---

## Decision #77: Restoring Tool Boundaries

**Date:** March 5, 2026
**Status:** APPROVED
**Category:** Process

### Decision
Explicitly re-establish the boundaries defined in Decision #53 after observing drift. The surgeon tool had been taking on full page builds (4+ files), which is the implementer's role. Restore the boundary and document it more precisely in the project instructions.

### What Triggered This
A full page was built by the wrong tool. The build technically worked but did not match established codebase patterns because the tool lacked full codebase context. The rework -- re-explaining the context, debugging mismatches, re-handoffing to the correct tool, reviewing the rebuild -- cost approximately 4 hours of orchestration time. For reference, the AI tools themselves execute builds in minutes; the human hours are spent in the back-and-forth of planning, reviewing, catching what's wrong, and iterating until it's right.

### Rules Reinforced
Added "Code Tool Trigger" to the decision tree with explicit routing criteria. Updated project instructions with the tool boundary rule in the Critical Reminders section.

### Lesson
Even documented decisions drift over time if not reinforced. Process decisions need periodic review, not just initial documentation. Decision #77 exists because Decision #53 was made but gradually forgotten.

---

*8 of 78 strategic decisions, selected to illustrate the documentation pattern.*
*Decisions span November 2025 -- March 2026, covering architecture, process, UX, and AI design.*
