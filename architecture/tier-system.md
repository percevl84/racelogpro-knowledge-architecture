# Tier System

## Prioritizing 140+ Documents for AI Context Loading

---

## The Problem

At 140+ documents totaling 2.5 MB, an AI tool cannot read everything at the start of a session. But it needs to know what matters. The tier system solves this by pre-classifying every document into one of four urgency levels, so the AI follows a simple reading rule instead of making 140 individual relevance judgments.

---

## The Four Tiers

### Critical Tier
**When to read:** Every session, always. No exceptions.
**What belongs here:** Documents that provide essential project context regardless of the current task.
**Typical count:** 3-7 files
**Typical size:** Under 10 KB each (combined ~30 KB)

Critical tier files answer: "What is the project, where does it stand, and what are we building?"

Examples:
- The current task queue (what to build next)
- The bug tracker (known issues)
- Locked page specifications for features currently under development
- The cross-page wiring framework (how pages interact)

### High Tier
**When to read:** Most sessions, or when the task matches the document's domain.
**What belongs here:** Documents that provide deep context for common task categories.
**Typical count:** ~15 files
**Typical size:** 10-50 KB each

High tier files answer: "What are the detailed specifications and patterns for this type of work?"

Examples:
- Design token definitions (read for any UI work)
- Data contract (read for any build work)
- AI prompt architecture (read for any coaching work)
- Handoff protocol template (read when writing handoffs)
- Completion reports from recent phases (read for context)

### Medium Tier
**When to read:** Only when building or modifying that specific feature.
**What belongs here:** Feature-specific specifications, infrastructure docs, and deep reference material.
**Typical count:** ~25 files
**Typical size:** 20-135 KB each

Medium tier files answer: "What are the exhaustive requirements for this particular feature?"

Examples:
- Full feature specifications (driver profile, track analysis, coaching flows)
- Infrastructure specifications (data integrity, code quality, multi-source import)
- AI naturalness rules (only relevant when tuning AI voice)

### Low Tier
**When to read:** Rarely. Historical reference only.
**What belongs here:** Completed handoffs, session summaries, superseded documents, archive material.
**Typical count:** 100+ files
**Typical size:** Varies (1-50 KB)

Low tier files answer: "What happened in the past?" -- a question rarely needed for current work.

Examples:
- Completed handoff documents from past phases
- Session summaries from weeks ago
- Superseded specifications replaced by newer versions
- Historical status reports

---

## How Tiers Are Assigned

### Initial Assignment
When a document is created, its tier is determined by its role:
- New phase plan for the current phase -> Critical
- New spec for a feature not yet built -> Medium
- New handoff document -> High (while active), Low (after completion)
- New session summary -> Low (immediately)

### Tier Promotion
Documents move UP in tier when they become relevant to active work:
- A Medium-tier spec becomes Critical when its feature enters the build phase
- A Low-tier completion report becomes High when its phase is being referenced for a new build

### Tier Demotion
Documents move DOWN in tier when their immediate relevance passes:
- A Critical phase plan becomes Low when the phase is complete
- A High-tier handoff becomes Low when the build is finished and tested
- A Critical bug tracker entry is removed when the bug is fixed

### Staleness Marking
Documents that are still in a high tier but have not been updated recently are marked as stale in the registry. This warns the AI to cross-reference with more current sources:

```
| H5 | Development Roadmap | 26 KB | STALE (Jan 30) -- use for historical phases only |
```

A stale document is not automatically demoted -- it may still contain useful information. But the staleness flag tells the AI not to trust it as the current source of truth.

---

## Tier Rules in Practice

### Rule: Critical Tier Is Small
If the Critical tier exceeds 7-8 files or 40 KB combined, it is too large. Re-evaluate: is everything truly needed every session, or are some files only needed for specific tasks?

### Rule: Tiers Are Not Quality Judgments
A Low-tier document is not less important than a Critical one. It is less relevant to current work. The 135 KB AI personality specification is Low-tier during a data pipeline task and would be Critical during AI voice tuning. Tier is about urgency, not value.

### Rule: Task Determines Additional Tiers
The registry's lookup table maps tasks to specific High and Medium tier files. This removes the AI's need to evaluate relevance document-by-document. The table says: "For UI work, read these 7 files. For data pipeline work, read these 3 files."

### Rule: One Person Maintains Tiers
Tier assignments are maintained by the strategist tool as part of the Document Registry. They are updated when phases complete, new documents are created, or the active focus shifts. This prevents the tiers from becoming stale through distributed ownership.

---

## Why Not Tags?

Tags (e.g., "AI," "UI," "data," "process") would also allow filtering. Tags were considered and rejected for two reasons:

1. **Tags require evaluation.** The AI must read each tag and decide if it is relevant to the current task. Tiers provide a pre-computed reading order: read Critical, check the lookup table for High/Medium, ignore Low.

2. **Tags do not express urgency.** A document tagged "AI" could be the active prompt spec (Critical) or a completed coaching flow spec (Low). Tags express domain, not urgency. Tiers express urgency. The lookup table handles domain routing.

---

## Results

| Metric | Before Tiers | After Tiers |
|--------|-------------|-------------|
| Documents AI reads per session | All (~70 KB minimum) | Critical + task-relevant (~30-90 KB) |
| Time to relevant context | 5-10 minutes | Under 1 minute |
| Irrelevant context loaded | ~50% of loaded content | Under 10% |
| "AI gave wrong answer because it read the wrong spec" | ~Weekly | Not observed since adoption |

The tier system reduced both context load size and error rate simultaneously -- less data loaded, but more relevant data loaded.

---

*Pattern developed during RaceLogPro development, February 2026.*
*Introduced alongside the Document Registry as the project exceeded 100 documents.*
