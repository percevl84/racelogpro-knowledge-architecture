# Document Registry System

## The Library Catalog Pattern for AI-Assisted Development

---

## The Problem

At 20 documents, you can paste everything into an AI's context window. At 140+ documents totaling 2.5 MB, you cannot. But the AI still needs to know what exists and where to find it.

The naive solution -- a single "master reference" file containing everything important -- works until it does not. Our master reference grew to 70 KB and became the bottleneck: too large to read every session, too interconnected to split, and increasingly stale because updating a 70 KB file is a chore nobody does consistently.

---

## The Solution: A Registry, Not a Reference

The Document Registry is a single index file (~22 KB) that serves as a library catalog for the entire project. It does not contain the information -- it tells the AI where the information lives and when to read it.

### What the Registry Contains

**Tier assignments** for every document in the project:

| Tier | When to Read | Typical Count |
|------|-------------|---------------|
| Critical | Every session, always | 3-7 files |
| High | Most sessions, or when task-relevant | ~15 files |
| Medium | Only when building that specific feature | ~25 files |
| Low | Rarely -- historical reference only | 100+ files |

**File metadata** for each document: path, size, one-line summary, and "read when" guidance.

**A lookup table** mapping common tasks to the exact files needed:

```
Task: "Writing a handoff for the build tool"
Read: Handoff Protocol + relevant spec + smoke test template

Task: "Investigating a bug"
Read: Bug backlog + gap analysis

Task: "UI work on any page"
Read: Design tokens (5 files) + data contract + relevant page spec
```

**A section index** for oversized documents, so the AI can read only the relevant section of a 70 KB file instead of the entire thing.

---

## How It Works in Practice

### Session Start (Every Session)
1. AI reads the Document Registry (~22 KB)
2. AI reads the Current State Snapshot (~6 KB)
3. AI now knows: what the project is, where it stands, and where every document lives
4. Total startup context: ~28 KB instead of the old ~70 KB

### During the Session
When a task requires specific knowledge, the AI consults the registry's lookup table and reads only the relevant files. A UI task pulls in design tokens. A data pipeline question pulls in the architecture spec. An AI coaching question pulls in the prompt architecture doc.

### Why This Works
- **Startup is fast.** 28 KB reads in seconds, not minutes.
- **Context is relevant.** The AI reads what it needs, not everything.
- **Maintenance is manageable.** Updating the registry (file list, tier assignments) is much easier than updating a monolithic reference document.
- **New tools onboard instantly.** Any AI tool that reads the registry can navigate the full document system.

---

## Registry Structure (Annotated)

```
# Document Registry v1.1

## Tier Definitions
  [Table: tier name, when to read, typical size, count]

## Critical Tier (Always Read)
  [7 files with path, size, summary]
  Combined read: ~30 KB

## High Tier (Read When Task-Relevant)
  [~15 files organized by category]
  Categories: Current State, Planning, AI/Coaching, Design, Protocols

## Medium Tier (Feature-Specific Reference)
  [~25 files for deep feature work]

## Low Tier (Historical / Rarely Needed)
  [100+ files: completed handoffs, session summaries, archives]

## Master Reference Section Index
  [Table: section number, heading, "use when" guidance]
  Enables targeted reads of the 70 KB file

## What to Read by Task
  [Lookup table: 12 common tasks -> exact file list]

## Documents Needing Updates
  [Maintenance queue for stale docs]
```

---

## Key Design Decisions

### Why tiers instead of tags or folders?
Tiers create a natural reading order. Tags require the AI to evaluate relevance for each document. Tiers pre-compute that evaluation: "If you are starting a session, read Critical. If you are doing UI work, also read these High-tier design files." The AI follows a simple rule instead of making 140 judgment calls.

### Why a separate registry instead of folder-based organization?
The folder structure organizes by purpose (specs, planning, status). The registry organizes by urgency and relevance. These are orthogonal. A spec might be Critical (the one you are building right now) or Low (a completed feature's historical spec). Folders cannot express this; tiers can.

### Why include file sizes?
AI tools have context limits. Knowing that a file is 135 KB vs. 3 KB determines whether you read it whole or read a specific section. Size metadata enables this decision without opening the file first.

### Why a lookup table?
Because "what should I read?" is the most common question at session start, and the answer should not require the AI to scan the entire registry. The lookup table is an index on top of the index -- O(1) access to the right reading list for any common task.

---

## Maintenance

The registry is updated when:
- A new document is created (add to appropriate tier)
- A phase completes (demote completed specs from Critical to Low)
- A document becomes stale (note in the registry, not just in the document)
- The lookup table no longer covers common tasks

One person maintains the registry. In this project, that is the strategy tool -- the same AI that does planning also maintains the index. This keeps the registry current without adding a separate maintenance step.

---

## Results

| Metric | Before Registry | After Registry |
|--------|----------------|----------------|
| Startup context load | ~70 KB (one monolithic file) | ~28 KB (registry + snapshot) |
| Time to relevant context | Minutes (reading everything) | Seconds (targeted reads) |
| Stale information risk | High (70 KB file rarely updated) | Low (small files updated individually) |
| New tool onboarding | Read everything and hope | Read registry, follow instructions |
| Document count supported | ~30 before it broke | 140+ and growing |

---

*Pattern developed during RaceLogPro development, February 2026.*
*Solves the "too many documents for AI context" problem that emerges at ~50+ project documents.*
