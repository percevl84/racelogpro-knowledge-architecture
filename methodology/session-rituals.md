# Session Rituals

## Structured Patterns for AI Work Sessions

---

## Why Rituals Matter

AI conversations drift. Not dramatically -- subtly. Each response is slightly less grounded than the last. Over a 50-message session, the AI has gradually shifted from building what you specified to building what it inferred. The rituals documented here exist to prevent that drift.

They are not ceremonies. They are mechanical checkpoints that keep the work aligned with the source documents.

---

## Session Start Ritual

Every new AI session begins with the same sequence:

### 1. Context Load
The AI reads two files before doing anything else:
- **Document Registry** (~22 KB) -- the index of all project documents
- **Current State Snapshot** (~6 KB) -- the 60-second project briefing

This gives the AI full project orientation in under 30 KB of context.

### 2. Focus Declaration
The human states the session's purpose:
- What we are building or solving
- Which phase or feature this relates to
- Any specific files or specs that are relevant

### 3. AI Confirmation
The AI confirms it understands the current state, the task, and has identified which additional documents it needs to read (using the registry's lookup table).

### Why This Exists
Without a start ritual, the AI guesses at context from the conversation. With it, the AI is grounded in documented reality before the first real question.

---

## Anchor Check

Use mid-session when something feels off -- the AI seems confused, contradicts a previous decision, or is solving the wrong problem.

### The Pattern
```
Anchor Check:
1. Re-read the Current State Snapshot (fresh, not from memory)
2. Confirm: What are we building? What is done? What is next?
3. Then answer the current question from that grounded state.
```

### When to Trigger
- The AI's response contradicts a spec or previous decision
- You have been going back and forth on the same point
- The conversation has been running for 30+ messages
- The AI says "as I mentioned earlier" but what it said was wrong
- You feel confused about what the AI thinks is happening

### Why This Exists
Context windows degrade over long conversations. The AI's attention to early messages weakens as the conversation grows. An anchor check forces a fresh read of the source of truth, resetting the AI's understanding to documented reality rather than accumulated conversational drift.

---

## Decision Logging

When an important decision is made during a session, document it immediately:

### The Pattern
```
Decision Made:
- What: [The decision]
- Why: [The rationale]
- Affects: [Which specs, code, or future work this impacts]
```

### What Counts as "Important"
- Choosing between two architectural approaches
- Deciding what is in scope vs. out of scope
- Changing a previously documented decision
- Establishing a pattern that future work should follow
- Any choice you would regret forgetting next week

### Why This Exists
Decisions made in conversation vanish when the chat closes. If they are not written down during the session, they are lost. The project accumulated 78 strategic decisions over 255+ hours. Every one was logged in-session, not reconstructed later.

---

## Session End Ritual

Every productive session ends with the same sequence:

### 1. Summary
The AI produces:
- What was accomplished this session
- Decisions made (with references to decision log entries)
- Documents that need updating
- Code changes made (if any)

### 2. Document Updates
Any changed state is written to the appropriate files:
- Current State Snapshot updated with new status
- Document Registry updated if new documents were created
- Bug tracker updated if bugs were found or fixed

### 3. Version Control
Exact commit commands are provided, ready to copy-paste:
```
cd C:\docs
git add .
git commit -m "docs: [what changed]"
git push
```

### 4. Next Steps
Clear statement of what the next session should start with.

### Why This Exists
The end ritual ensures that the next session starts from an accurate snapshot. Without it, the snapshot drifts from reality, and the start ritual loads stale context. The chain is only as strong as the end ritual's discipline.

---

## Red Flags (Start a New Session)

Certain signals mean the current session is degraded and a fresh one will be more productive:

- **AI contradicts a spec** -- it is working from conversational context, not documents
- **Same question asked twice** -- context is not being retained within the session
- **Circular conversation** -- the AI is pattern-matching rather than reasoning
- **50+ messages** -- long sessions degrade context quality
- **"I don't have access to..."** -- file loading has failed silently
- **References an old version** -- the AI is using training data, not your documents

When any of these appear, end the session cleanly (end ritual), then start fresh. A new session with proper context loading is always better than a degraded long session.

---

## Daily Workflow

```
+-- Start of Day --+
|                   |
| 1. Start new chat |
| 2. Run start ritual (context load + focus declaration) |
| 3. Do focused work |
| 4. Anchor check if drift detected |
| 5. Log decisions as they happen |
| 6. Run end ritual (summary + updates + commit) |
|                   |
+-- End of Day ----+
```

The rituals add approximately 5 minutes of overhead per session. They save approximately 30-60 minutes of confused, ungrounded work per session. The ROI is immediate and compounds over time.

---

*Pattern developed during RaceLogPro development, December 2025 -- March 2026.*
*Evolved from informal habits into documented protocols after repeated context drift incidents.*
