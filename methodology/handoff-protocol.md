# Handoff Protocol

## How Work Moves Between AI Tools Without Losing Context

---

## The Problem

When one AI tool plans the work and another builds it, the gap between "what was intended" and "what gets built" is where quality dies. The planning tool understands the full context -- the design philosophy, the 30 related decisions, the three previous attempts that failed. The building tool sees only what you put in the handoff document.

Poor handoffs produce technically correct code that solves the wrong problem.

---

## The Handoff Document

Every piece of work that moves from one AI tool to another is accompanied by a structured handoff document. This is not a suggestion or a template that gets skipped when you are in a hurry. It is mandatory, every time.

### Required Sections

**What It Does and Why**
Not just the feature description -- the purpose. Why this matters, what user problem it solves, which design decisions led here. The building tool needs to understand intent, not just requirements.

**Relevant Specifications**
Exact file references to the specs that govern this work. Not "see the design spec" but "see Design-Tokens-v1-Colors.md for the color palette, specifically the gold accent (#c9a84c) and the background (#0f0f17)."

**Files to Create or Modify**
An explicit list. No ambiguity about scope.

**Acceptance Criteria**
What "done" looks like. Observable, testable statements.

**What NOT to Change**
Equally important. When a building tool has full codebase access, it can "helpfully" refactor things outside the task scope. This section draws the boundary.

**Smoke Test Checklist**
Mandatory. See the [Smoke Test Framework](../workflow/smoke-test-framework.md) for details.

---

## The Preview-Before-Build Pattern

For any work that has a visual component (UI pages, components, layouts), the handoff includes an additional step:

### 1. Define
Strategist tool specifies what the component does, why, and which specs govern it.

### 2. Preview
Strategist generates an interactive HTML preview -- a clickable mockup that shows the intended result without touching the real codebase.

### 3. Iterate
The human reviews the preview: "Move this here," "Wrong feel," "The spacing is off." This takes minutes, not hours. Changes to an HTML preview are cheap. Changes to a React component wired to a backend are expensive.

### 4. Approve
The human confirms the preview matches their intent.

### 5. Write Handoff
The approved preview becomes the visual reference in the handoff document. The building tool now has both written specs AND a visual target.

### When to Skip the Preview
- Backend-only work (no visual component)
- Bug fixes (the correct behavior is already defined)
- Schema changes (data structure, not UI)
- Refactors (same behavior, different implementation)

---

## The Regression Rule

If a fix changes how data is read, written, deleted, or displayed, the building tool must check every page that uses that data. Not just the page being fixed -- every consumer of that data.

This rule exists because of a pattern that repeated three times before the rule was written: fix one thing, break three others. A database import fix that changes how sessions are stored can break the dashboard, the session list, the profile page, and the AI coaching context -- all of which read session data.

The rule is simple: **if you changed how data flows, verify everywhere it flows to.**

---

## Done Criteria

The building tool cannot declare work complete without a structured completion report:

```
SMOKE TEST RESULTS:
- Terminal Health:  PASS/FAIL (details if fail)
- Page 1:          PASS/FAIL
- Page 2:          PASS/FAIL
- Page 3:          PASS/FAIL
- Page 4:          PASS/FAIL
- Settings:        PASS/FAIL
- AI Chat:         PASS/FAIL

All tests passed. Task complete.
```

If any test fails, the building tool must fix it before stopping work. "Done with known failures" is not done.

---

## Handoff Sizing

Not all work needs the same depth of handoff:

| Work Type | Handoff Size | Tool |
|-----------|-------------|------|
| Full page build (4+ files) | 15-45 KB comprehensive handoff | Primary build tool |
| Targeted fix (1-3 files) | 2-5 KB focused instructions | Surgical fix tool |
| Large document edit (50KB+) | 1-2 line edit instruction | Document surgery tool |
| Bug fix with clear reproduction | 1-3 KB with steps to reproduce | Either tool |

The handoff should be as long as it needs to be and no longer. A 44 KB handoff for a full page build is appropriate. A 44 KB handoff for a one-line bug fix is waste.

---

## What Good Handoffs Prevent

- **Scope creep:** The building tool knows exactly what to change and what to leave alone
- **Regression:** The smoke test catches breaks before they reach the human
- **Context loss:** The building tool understands WHY, not just WHAT
- **Rework:** The preview catches visual mismatches before code is written
- **Ambiguity:** Acceptance criteria define done objectively

---

## Evolution

This protocol was not the first approach. The first approach was "tell the building tool what to do in a chat message." This worked for simple tasks and failed catastrophically for anything involving multiple files, existing patterns, or visual design.

The protocol evolved through failure:
1. **No handoff** -- building tool guesses at context, builds wrong thing
2. **Informal handoff** -- "fix the import bug" -- building tool fixes the wrong import bug
3. **Structured handoff without smoke test** -- correct fix, but breaks three other things
4. **Structured handoff with smoke test** -- correct fix, verified, no regressions
5. **Preview + structured handoff + smoke test** -- correct design, correct implementation, verified

Each layer was added because the previous level was not sufficient. The current protocol is the minimum viable process that produces reliable results.

---

*Pattern developed during RaceLogPro development, February 2026.*
*Evolved through multiple handoff failures into a mandatory protocol.*
