# Current State Snapshot Pattern

## The 60-Second Briefing for AI Context

---

## The Problem

Every AI session starts from zero. The AI has no memory of yesterday's breakthrough, last week's architectural decision, or the bug you fixed three hours ago. You need to bring it up to speed -- fast, accurately, and without drowning it in irrelevant history.

A master reference file tries to be comprehensive. A project status tracker tries to be complete. Neither is optimized for what an AI actually needs at session start: the minimum viable context to be productive in under 60 seconds.

---

## The Solution: A Snapshot, Not a History

The Current State Snapshot is a single file (~4-6 KB) designed to answer one question: "If an AI reads only this file, can it start working immediately?"

It is updated after every productive session. It is not a log -- it is a photograph of right now.

---

## What the Snapshot Contains

### App State Table
A compact key-value table with the essential facts:

```
| Item              | Value                                        |
|-------------------|----------------------------------------------|
| App Version       | v1.1.0                                       |
| Current Phase     | Phase 10D 95% -- Phase B done, C remaining   |
| Database          | 316 sessions, 181 cars, 452 tracks           |
| Total Hours       | ~255                                         |
| Strategic Decisions | 78 total                                   |
| Last Completed    | Phase B -- AI Content Generation              |
```

An AI reading this table knows the project maturity, scale, and current focus in 10 seconds.

### What It Looks Like Right Now
A narrative description of the current user experience. Not what is planned -- what exists. This grounds the AI in reality:

```
"The home page shows a real AI-generated greeting, real session data,
and context-aware chat suggestions. The week plan tab generates
AI day-by-day plans with status badges. Four bugs remain: career path
uses demo data, stat cards show placeholders, header has a data
mismatch, and settings page styling needs work."
```

### Build Status
A checkbox list showing the current phase's progress:

```
- [x] All 4 visual shells built
- [x] Backend wiring pass -- 23 IPC channels
- [x] AI content generation -- 2 new files, 7 modified
- [ ] Polish + remaining gaps (NEXT)
- [ ] Integration testing
```

### Known Bugs
A short list of open issues, sourced from the bug tracker but summarized here for quick reference.

### Key Code Paths
A lookup table mapping concepts to file paths:

```
| What              | Path                          |
|-------------------|-------------------------------|
| Main process      | electron/main.js              |
| Services          | electron/services/*.cjs       |
| React pages       | src/pages/*.jsx               |
| Database          | racelogpro_v2.db              |
```

### Top Gotchas
A list of things that will waste an hour if you do not know them:

```
1. Edit electron/main.js -- NEVER dist/main/main.js
2. PowerShell uses ; not && for chaining commands
3. Close the DB browser before running the app
4. Tables use track_id/car_id -- NOT id
5. Services use .cjs extension (CommonJS)
```

### Session Rituals
A three-line reminder of how sessions should start, anchor, and end.

---

## What the Snapshot Does NOT Contain

- **History.** It does not explain how we got here. That is the master reference's job (read only when needed).
- **Specs.** It does not duplicate feature specifications. It points to them.
- **Every bug.** It lists only the currently open ones, not the resolved backlog.
- **Architecture.** It does not explain the tech stack in depth. It provides just enough for orientation.

The snapshot is a briefing, not a textbook.

---

## Update Discipline

The snapshot is updated at the end of every productive session. This is non-negotiable. The update is small -- usually changing 3-5 values in the app state table, updating the build status checkboxes, and adjusting the "what it looks like" narrative.

The AI strategist that runs the session is also the one that updates the snapshot before closing. This keeps the update in the natural workflow instead of being a separate chore.

### What Gets Updated
- App state values (version, phase, counts)
- Build status checkboxes
- "What it looks like" narrative (if anything changed visually)
- Known bugs (add new, remove resolved)
- Key code paths (if new files were created)

### What Stays Stable
- Top gotchas (only change when a new gotcha is discovered)
- Tech stack (changes rarely)
- Session rituals (structural, not session-specific)

---

## Why This Works

### It Is Small Enough to Read Every Time
At 4-6 KB, the snapshot fits comfortably alongside the Document Registry (~22 KB) in any AI context window. Combined startup context is under 30 KB.

### It Is Current Enough to Trust
Because it is updated every session, the information is at most one session old. Compare this to a master reference that might be weeks stale.

### It Answers the Right Questions
An AI starting a session needs to know: What is the project? Where does it stand? What was just done? What is next? What will trip me up? The snapshot answers all five in under 60 seconds of reading.

### It Replaces Verbal Context-Setting
Without a snapshot, every session starts with the human explaining "OK so here is where we are..." for 5-10 minutes. The snapshot eliminates this overhead entirely.

---

## Relationship to Other Documents

```
Document Registry  -->  "Here is everything that exists and where to find it"
State Snapshot     -->  "Here is where we are RIGHT NOW"
Master Reference   -->  "Here is the deep history and architecture"
                        (read sections on demand, not every session)
```

The Registry and Snapshot together form the "always read" pair. They replaced the monolithic master reference as the session-start reading material.

---

*Pattern developed during RaceLogPro development, February 2026.*
*Solves the "AI has no memory between sessions" problem by making the briefing fast, accurate, and always current.*
