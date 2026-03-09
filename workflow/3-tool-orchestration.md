# 3-Tool Orchestration

## Defining Roles for AI Tools Working on the Same Codebase

---

## The Problem

Using one AI tool for everything is like having one person be the architect, the contractor, and the electrician. It works for a shed. It does not work for a house.

AI tools have different strengths. Some excel at strategic planning and long-form reasoning. Some excel at code generation with full codebase awareness. Some excel at targeted, surgical fixes. Using the right tool for the right job is not just efficient -- it prevents the kind of errors that happen when a planning tool tries to write production code or a code generation tool tries to make architectural decisions.

---

## The Three Roles

### Tool 1: The Strategist
**Role:** Planning, decisions, specifications, handoff documents, debugging conversations, understanding work.

**Strengths:**
- Long-context reasoning across 140+ documents
- Strategic decision-making with full project history
- Writing comprehensive handoff documents
- Generating HTML previews for design iteration
- Maintaining the document system (registry, snapshot, decisions)

**Uses:**
- "What should we build next?"
- "Why does this architecture work this way?"
- "Write the handoff for the next feature."
- "Here is a bug -- help me understand it before fixing it."

**Does not do:** Write production code, touch the codebase directly.

### Tool 2: The Implementer
**Role:** Primary code builder. Full page builds, multi-file features, major refactors.

**Strengths:**
- Full codebase indexing -- sees all existing components, services, patterns
- Can create and modify 10+ files in a single operation
- Understands how new code fits into existing architecture
- Multi-agent workflows for complex builds

**Uses:**
- "Build this full page from the handoff document."
- "Wire these 23 IPC channels across 4 pages."
- "Refactor the service layer to support this new pattern."

**Does not do:** Strategic planning, document maintenance, quick one-off fixes.

### Tool 3: The Surgeon
**Role:** Targeted fixes (1-3 files) and large document surgery (50KB+ files).

**Strengths:**
- Fast, focused edits without codebase overhead
- Can handle files too large for other tools to edit reliably
- Precise: changes exactly what is specified, nothing more

**Uses:**
- "Fix this specific bug in this specific file."
- "Update Section 7 of this 70 KB reference document."
- "Change these 3 lines in the main process file."

**Does not do:** Full page builds, multi-file features, strategic decisions.

---

## The Decision Tree

When work needs to happen, the routing is mechanical, not subjective:

```
"What should we build?"           -> Strategist
"Build this big thing"            -> Implementer
"Fix these large docs"            -> Surgeon
"Why doesn't this work?"          -> Strategist
"Small 1-3 file fix"             -> Surgeon
"Full page / 4+ files"           -> Implementer

More precisely:

Build touches 4+ files?          -> Implementer
Full page build?                  -> Implementer
Codebase context needed?          -> Implementer
1-3 files, targeted fix?          -> Surgeon
50KB+ document edit?              -> Surgeon
Planning / decisions / specs?     -> Strategist
```

---

## How They Communicate

The tools do not talk to each other directly. They communicate through documents:

### Strategist -> Implementer
Via handoff documents (15-45 KB). Contains intent, specs, file lists, acceptance criteria, smoke test, and what not to change. See [Handoff Protocol](../methodology/handoff-protocol.md).

### Strategist -> Surgeon
Via focused instructions (1-5 KB). Contains the exact edit, which file, and why.

### Implementer -> Strategist
Via completion reports with smoke test results. Strategist reviews and updates project state documents.

### Surgeon -> Strategist
Via commit messages and brief completion notes. Strategist updates the snapshot.

```
Strategist                    Implementer
    |                              |
    |--- Handoff Document -------->|
    |                              |--- Builds --->
    |<-- Completion Report --------|
    |                              |
    |--- Updates Snapshot          |
    |--- Updates Registry          |


Strategist                    Surgeon
    |                              |
    |--- Edit Instructions ------->|
    |                              |--- Fixes --->
    |<-- Commit Confirmation ------|
    |                              |
    |--- Updates Snapshot          |
```

---

## Why Not Just One Tool?

### The Strategist Cannot Build
It lacks codebase awareness. It does not see how existing components are structured, what patterns are established, or where new code should integrate. Its code suggestions are plausible but disconnected from reality.

### The Implementer Cannot Plan
It optimizes for code quality in the current task, not strategic coherence across the project. It will happily build a feature that contradicts a decision made three weeks ago, because it does not read the decision log.

### The Surgeon Cannot Architect
It is optimized for precision, not breadth. Asking it to build a full page is like asking a heart surgeon to design a hospital. Different skill, different scope.

---

## Common Mistakes (Learned the Hard Way)

### Using the Surgeon for Full Page Builds
The surgeon works without full codebase context. It does not see existing component patterns, shared utilities, or established conventions. A full page built by the surgeon will technically work but will not match the codebase's style or reuse existing abstractions.

**Fix:** Full page builds and 4+ file changes always go to the implementer.

### Using the Implementer for Quick Fixes
The implementer's strength is comprehensive builds. Using it for a 3-line bug fix wastes its codebase indexing overhead and risks unnecessary refactoring of adjacent code.

**Fix:** Targeted 1-3 file fixes go to the surgeon.

### Skipping the Strategist
Jumping straight to implementation feels faster. It is faster for the first session. By the fifth session, the accumulated drift from unplanned decisions, undocumented choices, and inconsistent direction costs more than the planning would have.

**Fix:** Every significant feature goes through the strategist first, even if it adds a session of overhead.

### Letting Roles Drift
Over time, the boundaries blur. The strategist starts writing code snippets. The implementer starts making architectural decisions. The surgeon takes on multi-file features. Each drift is small and reasonable in the moment. The cumulative effect is that no tool is doing its job well.

**Fix:** Explicit role documentation, reviewed periodically. This was formalized as Strategic Decision #77 after a drift incident.

---

## Results

The 3-tool workflow was adopted formally in January 2026. Since then:
- 4 full page builds completed (each 4-14 files)
- 23 IPC channels wired in a single coordinated build pass
- AI content generation layer added across all pages
- Zero "built the wrong thing" incidents (previously ~1 per week)
- Handoff document quality directly correlates with build quality

The overhead of writing handoff documents and maintaining tool boundaries is real (~15-20% of total time). The reduction in rework, misalignment, and debugging more than compensates.

---

*Pattern developed during RaceLogPro development, January -- March 2026.*
*Formalized as Strategic Decision #53 after experiencing the problems of single-tool development.*
