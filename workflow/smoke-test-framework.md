# Smoke Test Framework

## Quality Assurance for AI-Built Code

---

## The Problem

AI code generation tools are confident. They will build exactly what you asked for, declare it complete, and move on. The problem is that "complete" often means "the new thing works" without verifying that the old things still work.

In a desktop application with multiple pages sharing a database, a change to data import can silently break the dashboard, the session list, the profile page, and the AI coaching context. The AI tool does not check these pages unless explicitly required to.

---

## The Smoke Test

Every handoff document includes a pass/fail checklist that the building tool must execute before declaring the work complete. This is not optional. This is not "run it if you have time." This is a hard requirement.

### What the Checklist Covers

The checklist tests every major surface of the application, not just the feature being built:

**Terminal Health**
- No database errors in the console
- No red errors (known warnings excepted)
- All backend services initialize without errors
- Data queries return results (not empty)

**Each Page of the Application**
For every page, the checklist specifies what "working" looks like. Not "renders without crashing" but specific, observable outcomes:

```
Page 1:
- Chart renders with actual data points (not "No data")
- List shows items with real names and numbers
- Numbers are plausible (not -1, not 0, not placeholder)
- Color coding works (positive green, negative red)

Page 2:
- Default view shows data (not empty state)
- Filters work (changing filter changes results)
- Each row has all expected fields populated
- Fields contain real values, not placeholders

Page 3:
- Key metrics show numbers (not "--")
- Categories show real labels (not "--")
- All sections populated

Page 4:
- Core functionality loads without errors
- Interactive elements respond
- No visual regressions
```

**Settings**
- Import/re-import works without database errors
- Configuration changes persist

**AI Integration**
- AI chat loads and responds
- AI features on every page still function

---

## The Regression Rule

The most important rule in the framework:

**If your fix changes how data is read, written, deleted, or displayed, check EVERY page that uses that data.**

This rule prevents the "fix one, break three" pattern. A database schema change can cascade across the entire application. The building tool must trace the data flow and verify each consumer.

### Example
You fix a bug in the session import handler. The import now correctly handles a new field. Before declaring done, you must verify:
- The dashboard still renders session data correctly
- The session list still shows all sessions with correct fields
- The profile page still computes stats from session data
- The AI coaching context still assembles session history properly

---

## Done Criteria

The building tool cannot declare work complete without producing a structured report:

```
SMOKE TEST RESULTS:
- Terminal:  PASS/FAIL (details if fail)
- Page 1:    PASS/FAIL
- Page 2:    PASS/FAIL
- Page 3:    PASS/FAIL
- Page 4:    PASS/FAIL
- Settings:  PASS/FAIL
- AI Chat:   PASS/FAIL

All tests passed. Task complete.
```

### Rules
- If ANY test fails, the building tool must fix it before stopping
- "Done with known failures" is NOT done
- The human should not accept a completion report with FAIL entries
- Every FAIL must include details about what failed and what was attempted

---

## Why Not Automated Tests?

Automated tests are better. This project does not have them yet (solo developer, AI-assisted, 255 hours of development). The smoke test checklist is the minimum viable quality assurance that can be executed by the building tool itself without a test framework.

The checklist pattern works because:
- It requires no setup or infrastructure
- The building tool can execute it immediately after building
- It catches the most common regressions (data display, page rendering, import integrity)
- It takes approximately 2 minutes to run
- It prevents approximately 2 hours of debugging per incident

When the project matures to the point where automated tests are warranted, the smoke test checklist becomes the specification for what those tests should verify.

---

## Checklist Evolution

The checklist grows as the application grows. When a new page is added, its verification criteria are added to the checklist. When a new data flow is introduced, its consumers are added to the regression check.

The checklist does not shrink. Features removed from the application have their checks removed, but features that still exist always get checked. There is no "we probably don't need to check that page anymore."

---

## Integration with Handoff Protocol

The smoke test is embedded in every handoff document, not maintained separately. This ensures:
- The building tool sees the checklist in the same document as the build instructions
- The checklist is tailored to the specific build (additional checks for the feature being built)
- There is no separate step to "remember to run the smoke test"

```
Handoff Document Structure:
1. What to build and why
2. Files to create/modify
3. Acceptance criteria for the new feature
4. What NOT to change
5. SMOKE TEST CHECKLIST  <-- Always present, always mandatory
6. Done criteria (structured report format)
```

---

*Pattern developed during RaceLogPro development, February 2026.*
*Created after three separate incidents where a fix broke unrelated features.*
*The 2-minute checklist has prevented every regression since its introduction.*
