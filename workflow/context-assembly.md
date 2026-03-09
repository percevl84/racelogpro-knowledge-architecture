# Context Assembly

## Building the Right Context Packet for Each AI Session

---

## The Problem

AI tools are only as good as the context you give them. Too little context and they guess. Too much context and they lose focus -- the relevant information drowns in noise. The challenge is not just having good documentation; it is assembling the right subset of documentation for each specific task.

---

## The Principle: Layered Context

Context is assembled in layers, from broadest orientation to narrowest task focus:

```
Layer 1: Project Orientation (always)
    "What is this project? Where does it stand?"

Layer 2: Task Routing (always)
    "What files and specs are relevant to this specific task?"

Layer 3: Deep Reference (on demand)
    "What are the detailed specs for this particular feature?"

Layer 4: Historical Context (rarely)
    "What decisions led to this architecture?"
```

Each layer adds context only when needed. Most sessions require Layers 1-2. Complex feature work adds Layer 3. Architectural questions add Layer 4.

---

## Layer 1: Project Orientation

**Read every session, no exceptions.**

| Document | Size | What It Provides |
|----------|------|------------------|
| Document Registry | ~22 KB | Map of all 140+ documents with tier assignments |
| Current State Snapshot | ~6 KB | 60-second briefing on project state |

**Combined: ~28 KB.** This fits in any AI context window and provides complete project orientation.

After reading these two files, the AI knows:
- What the project is and its current phase
- What was just completed and what is next
- Where every document lives and when to read it
- Known bugs and gotchas
- Key file paths in the codebase

---

## Layer 2: Task Routing

**Determined by the task, loaded from the registry's lookup table.**

The Document Registry includes a "What to Read by Task" section that maps common tasks to exact file lists:

| Task | Additional Files |
|------|-----------------|
| UI work on any page | Design tokens (5 files: colors, typography, spacing, components, index) + data contract |
| AI coaching integration | Prompt architecture spec + AI personality spec |
| Bug investigation | Bug backlog + gap analysis |
| Writing a handoff | Handoff protocol template + relevant page spec |
| Cross-page navigation | Wiring framework (ownership model, link map) |
| Data pipeline work | Data architecture spec + pipeline completion report |

This layer adds 10-60 KB depending on the task. The lookup table eliminates the AI's need to guess which files matter.

---

## Layer 3: Deep Reference

**Loaded only for complex feature work.**

When building or modifying a specific feature, the AI needs the full spec for that feature. These are Medium-tier documents in the registry, typically 20-135 KB each.

Examples:
- Building the driver profile page: load the Driver DNA Spec (123 KB)
- Refining AI coaching voice: load the AI Naturalness Spec (89 KB)
- Implementing data import: load the Multi-Source Import Spec (78 KB)

The registry's tier system prevents loading these for unrelated tasks. A bug fix on the settings page does not need the 123 KB Driver DNA Spec.

---

## Layer 4: Historical Context

**Loaded rarely, for architectural questions only.**

The strategic decisions document (85 KB) and the master reference sections are historical context. They explain WHY things are the way they are, not what to build next.

When to load Layer 4:
- "Why did we choose this database schema?"
- "What alternatives were considered for the AI architecture?"
- "Is this proposed change consistent with previous decisions?"

When NOT to load Layer 4:
- Any build task (Layers 1-3 are sufficient)
- Bug fixes (the bug does not care about history)
- UI work (specs define the target, not the history)

---

## Context Budget

Every AI tool has a context limit. Context assembly must stay within budget while maximizing relevance.

### Practical Budgets (Approximate)

| Layer | Typical Size | Cumulative |
|-------|-------------|------------|
| Layer 1 (always) | 28 KB | 28 KB |
| Layer 2 (task routing) | 10-60 KB | 40-90 KB |
| Layer 3 (deep reference) | 20-135 KB | 60-225 KB |
| Layer 4 (historical) | 50-85 KB | 110-310 KB |

Most sessions stay under 100 KB of context. This is well within the capacity of modern AI tools.

### When Budget Is Tight
If a task requires multiple large specs, the registry's section index allows loading specific sections of large files rather than entire documents. The 70 KB master reference, for example, has 12 indexed sections -- loading just the database schema section is 5 KB instead of 70 KB.

---

## Assembly Rules

### Rule: Never Guess at Context
If you are not sure whether a document is relevant, check the registry. The lookup table and tier system exist to eliminate guesswork.

### Rule: Read Specs, Not Summaries
Summaries lose critical detail. When a task requires a spec, load the actual spec. The 10 extra KB is always worth it compared to building from a summary and getting the details wrong.

### Rule: Current State Over Historical State
If the snapshot says Phase 10D is 95% complete, that supersedes whatever the master reference says about Phase 10. Always trust the most recently updated source.

### Rule: Load Before Starting
All relevant context must be loaded before the AI begins working on the task. Loading context mid-task after the AI has already formed an approach is less effective than loading it upfront.

---

## Anti-Patterns

### Loading Everything
"Just read all the Critical and High tier files to be safe." This wastes context budget and dilutes attention. A UI task does not benefit from the AI prompt architecture spec, even if both are High tier.

### Loading Nothing
"I will explain the context in the chat." This works for simple questions and fails for complex builds. The human's verbal explanation will miss details that the spec documents contain.

### Loading Stale Documents
A document marked as stale in the registry should not be trusted without cross-referencing the snapshot. Loading a stale roadmap for planning leads to building features that have already been superseded.

### Loading History for Build Tasks
Strategic decisions explain why things were decided. They do not help build them. Loading 85 KB of decision history for a UI component is pure noise.

---

*Pattern developed during RaceLogPro development, February 2026.*
*The layered approach emerged from observing which documents actually improved AI output quality for different task types.*
