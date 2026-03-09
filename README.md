# RaceLogPro Knowledge Architecture

**How one person with no CS degree used AI orchestration and a 140-document knowledge system to build a real desktop application.**

---

## What This Is

This repo documents the methodology and system design behind [RaceLogPro](https://github.com/percevl84), an AI-powered coaching application for sim racers. It is not the product codebase -- it is the knowledge architecture that made the product possible.

RaceLogPro is a desktop app (Electron + React) with a connected AI coaching engine, a 1,379-concept knowledge base drawn from 12 books, and a real-time data pipeline. It was built entirely through AI-assisted development over 255+ hours, producing 78 strategic decisions and 140+ structured documents.

This repo exists to show the patterns, not the product. If you are building with AI tools and struggling with context loss, session drift, or the gap between "AI can write code" and "AI can help me build a product," the system documented here might help.

---

## The Problem This Solves

AI coding assistants are powerful in a single session. But building a real product takes hundreds of sessions across weeks and months. The core challenges:

- **Context evaporates.** Every new chat starts from zero. Your AI has no memory of the 47 decisions you made last week.
- **Drift compounds.** Without grounding documents, each session drifts slightly from the last. Over 100+ sessions, you have built three different products.
- **Handoffs break.** Moving work between tools (or between an AI strategist and an AI implementer) loses critical context unless you structure the transfer explicitly.
- **Scale defeats memory.** At 20 documents, you can paste everything into context. At 140 documents and 2.5 MB of specs, you need a system.

---

## The Evolution

This system was not designed upfront. It grew organically as each problem surfaced, which is part of why it works -- every piece exists because something broke without it.

### Stage 1: Single Chat (November 2025)
Copy-pasting code into one Claude conversation. No structure, no files, no persistence. Each session started from scratch. Progress was real but fragile.

### Stage 2: Separated Concerns (December 2025)
Split strategy conversations from development work. Created separate project spaces for planning vs. building. First specs written. Decisions started getting documented instead of forgotten.

### Stage 3: Document Management (Late December 2025)
Realized that 20+ spec files needed organization. Created a folder structure. Started versioning documents. Wrote session rituals (start, anchor check, end) to keep AI grounded across chats.

### Stage 4: Connected Repos (January 2026)
Created a dedicated docs repository synced to all project spaces. 60+ documents across 7 folders. Strategic decisions tracked formally (reached 50+). Handoff protocols written for moving work between tools.

### Stage 5: The Registry System (February 2026)
At 100+ documents, the old approach of "read everything at session start" broke. A single reference file hit 70 KB -- too large for practical context loading. Built the Document Registry: a 22 KB index that tells AI tools exactly which files to read for any given task. Introduced the 4-tier reading system (Critical / High / Medium / Low).

### Stage 6: Multi-Tool Orchestration (February-March 2026)
Defined clear roles for three AI tools working on the same codebase. Wrote structured handoff documents with mandatory smoke tests. Built quality assurance frameworks to prevent regressions. The system reached 140+ documents, 78 strategic decisions, and a tiered reading system managing 2.5 MB of combined project knowledge.

---

## The System Today

### 7-Folder Structure
The document system is organized into purpose-driven folders. See [architecture/folder-structure.md](architecture/folder-structure.md) for details.

### 4-Tier Reading System
Not every document matters for every task. The tier system tells AI tools what to read and when. See [architecture/tier-system.md](architecture/tier-system.md).

### Document Registry
A single index file that serves as the "library catalog" for the entire project. AI reads this instead of everything. See [methodology/document-registry-system.md](methodology/document-registry-system.md).

### Current State Snapshot
A 60-second briefing that gives any AI tool full project context in under 6 KB. Updated after every productive session. See [methodology/state-snapshot-pattern.md](methodology/state-snapshot-pattern.md).

### Session Rituals
Structured patterns for starting, grounding, and ending AI work sessions. Prevents drift, ensures continuity. See [methodology/session-rituals.md](methodology/session-rituals.md).

### 3-Tool Orchestration
Three AI tools with defined roles: strategist, implementer, and surgeon. Clear decision tree for which tool handles which work. See [workflow/3-tool-orchestration.md](workflow/3-tool-orchestration.md).

### Handoff Protocol
How work moves between tools with mandatory quality checks. See [methodology/handoff-protocol.md](methodology/handoff-protocol.md).

### Smoke Test Framework
Every handoff includes a pass/fail checklist. No tool declares "done" until every test passes. See [workflow/smoke-test-framework.md](workflow/smoke-test-framework.md).

### Context Assembly
How context packets are built for each AI session -- the right information, at the right depth, for the right task. See [workflow/context-assembly.md](workflow/context-assembly.md).

### Strategic Decisions
78 decisions documented with rationale, alternatives considered, and downstream impact. See [decisions/](decisions/) for examples.

---

## A Note on Hours

The 255+ hours in this project are human orchestration hours -- not coding hours. AI tools execute builds in minutes that would take a traditional developer days. A full-page React component with backend wiring might take Cursor 15 minutes to generate. But the hours spent planning what to build, writing the handoff, reviewing the output, catching mismatches, debugging edge cases, re-explaining context when something drifts, and iterating until it is right -- that is where the 255 hours live.

This is the counterintuitive reality of AI-assisted development: the code generation is fast; the orchestration is the actual work. Writing a 44 KB handoff document that gives the implementer enough context to build correctly the first time takes longer than the build itself. But it is faster than the alternative: a quick handoff, a wrong build, and 4 hours of rework.

---

## By the Numbers

| Metric | Value |
|--------|-------|
| Development hours | 255+ |
| Strategic decisions documented | 78 |
| Project documents | 140+ |
| Knowledge base concepts | 1,379 (from 12 books) |
| Document system size | ~2.5 MB |
| Folder structure | 7 purpose-driven folders |
| Reading tiers | 4 (Critical / High / Medium / Low) |
| AI tools orchestrated | 3 with defined roles |
| Database records | 316 sessions, 181 cars, 452 tracks |
| Tech stack | Electron + React 19 + SQLite + Claude API + Python |

---

## What This Proves

Traditional software development assumes you either know how to code or you hire someone who does. AI-assisted development opens a third path, but it requires a different skill: **systems thinking applied to knowledge management.**

The orchestration IS the engineering skill. Knowing which AI tool to use, what context it needs, how to structure a handoff so nothing gets lost, how to prevent 140 documents from becoming 140 sources of contradiction -- that is the work.

This system was not built by someone with a CS degree. It was built by someone who spent 22 years as a professional ballroom dancer, where you learn that the beauty of a figure eight is not decoration applied on top -- it is the mechanics working correctly. Every document in this system exists because a mechanical problem needed solving. The structure is the product.

---

## Repo Structure

```
racelogpro-knowledge-architecture/
|-- README.md                         <- You are here
|-- methodology/
|   |-- document-registry-system.md   The tier system and how AI reads docs
|   |-- state-snapshot-pattern.md     60-second briefing concept
|   |-- session-rituals.md            Start / anchor / end patterns
|   `-- handoff-protocol.md           How work moves between tools
|-- workflow/
|   |-- 3-tool-orchestration.md       Claude / Cursor / Claude Code roles
|   |-- context-assembly.md           How context packets are built
|   `-- smoke-test-framework.md       Quality assurance
|-- decisions/
|   |-- README.md                     How decisions are documented
|   `-- example-decisions.md          8 sanitized strategic decisions
`-- architecture/
    |-- folder-structure.md           The 7-folder organization
    `-- tier-system.md                How docs are prioritized
```

---

## Who This Is For

- **Solo builders using AI tools** who have hit the wall where single-chat development stops scaling
- **Non-engineers building real products** who need patterns for managing complexity without a traditional engineering background
- **Anyone interested in AI orchestration** as a discipline -- not prompt engineering for one query, but sustained multi-tool collaboration across months of development
- **Hiring managers and collaborators** evaluating what "built with AI" actually means in practice

---

## About

Built by [Andrea Marletta](https://www.linkedin.com/in/marletta-ai/) -- professional ballroom dancer turned AI product builder, lifelong motorsport enthusiast, and someone who has never been able to separate the love of learning from the need to teach what he learns. RaceLogPro exists at that intersection: a real product born from a passion for racing, built through a system designed to make AI tools genuinely useful, and documented here because the methodology should not stay private.

Questions, feedback, or similar war stories welcome via [LinkedIn](https://www.linkedin.com/in/andreamarletta/) or [GitHub Issues](https://github.com/percevl84/racelogpro-knowledge-architecture/issues).

---

*Last updated: March 2026*
