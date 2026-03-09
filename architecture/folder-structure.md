# Folder Structure

## 7 Purpose-Driven Folders for 140+ Documents

---

## The Principle

Documents are organized by purpose, not by chronology or tool. A spec written in December lives next to a spec written in March if they serve the same purpose. A handoff document lives in Development, not in Planning, because its purpose is to drive a build -- even though it was written during a planning session.

---

## The Structure

```
docs/
|-- 01-Vision/
|-- 02-Specs/
|   |-- AI-Core/
|   |-- Features/
|   |-- Infrastructure/
|   `-- Data-Integration/
|-- 03-Knowledge-Base/
|-- 04-Planning/
|-- 05-Status/
|-- 06-Development/
|   |-- Handoffs/
|   |-- Backlog/
|   `-- Project-Instructions/
`-- 07-Archive/
    |-- Previews/
    `-- Superseded/
```

---

## Folder Purposes

### 01-Vision
**What lives here:** Product vision, north star documents, design philosophy.
**When to read:** Rarely. When questioning whether a feature aligns with the product's core purpose.
**Size:** Small. A few foundational documents.

### 02-Specs (with subcategories)
**What lives here:** All specifications, organized by domain.
**Subcategories:**
- **AI-Core/** -- Specifications governing the AI coaching engine: prompt architecture, personality, naturalness rules, coaching methodology, knowledge base structure
- **Features/** -- Specifications for individual product features: user profiles, track analysis, progression systems, onboarding flows
- **Infrastructure/** -- Technical specifications: design system, ecosystem architecture, code quality, data integrity
- **Data-Integration/** -- Specifications for data pipelines: external API integration, import processes, data models

**When to read:** When building or modifying the relevant feature. The Document Registry maps tasks to specific spec files.
**Size:** Largest folder. 40+ spec files ranging from 14 KB to 135 KB.

### 03-Knowledge-Base
**What lives here:** Management files for the curated knowledge base (extraction tracking, pillar definitions, concept categorization). Also contains the 80+ extraction files from 12 source books.
**When to read:** When working on knowledge base integration or coaching content.
**Size:** Large in aggregate (~80 extraction files) but individual management files are small.

### 04-Planning
**What lives here:** Phase plans, roadmaps, strategic decisions, design token definitions, page specifications (locked designs), data contracts.
**When to read:** Multiple files are Critical tier (read every session). Others are task-dependent.
**Size:** 20+ files. Phase plans and design tokens are frequently referenced.

### 05-Status
**What lives here:** Current state documents, the Document Registry itself, the Current State Snapshot, the master reference, completion reports, session summaries, bug trackers.
**When to read:** Registry and Snapshot are read every session. Completion reports and summaries are historical reference.
**Size:** 20+ files. Most are session-specific and become Low tier quickly.

### 06-Development
**What lives here:** Everything that drives builds. Handoff documents, Cursor/Claude Code mission files, build instructions, gap analyses, project instruction files.
**When to read:** When writing or executing a handoff. Historical handoffs are Low tier after completion.
**Size:** 30+ files. Each phase produces 3-8 handoff documents.

### 07-Archive
**What lives here:** Completed work that is no longer actively referenced. HTML preview files (locked designs that drove builds), superseded documents, early-phase artifacts.
**When to read:** When checking the visual reference for a completed page build. Otherwise, rarely.
**Size:** Varies. Locked HTML previews are the most frequently accessed archive items.

---

## Why This Structure

### Why not chronological?
Chronological organization (Phase-1/, Phase-2/, Phase-3/) makes finding the latest version of anything a search across multiple folders. Purpose-based organization means all specs live together regardless of when they were written, and the Document Registry handles "which is current?"

### Why subcategories in 02-Specs but not elsewhere?
Specs are the largest and most diverse category. Without subcategories, 40+ files in a flat list are hard to navigate. Other folders have fewer files and do not need the additional nesting.

### Why a separate Archive?
Files that are no longer actively referenced but might be needed for historical context (especially locked HTML previews used as visual references) need a home that does not clutter the active folders. Archive items are Low tier by default.

### Why Development separate from Planning?
Planning produces specs and decisions. Development produces handoffs and build instructions. A handoff references a plan but serves a different purpose: it drives immediate execution by a specific tool. Keeping them separate makes the Development folder a clean queue of "things to build" without mixing in strategic analysis.

---

## Growth Pattern

The folder structure has been stable since January 2026. New documents are added to existing folders, not new ones. The subcategory structure within 02-Specs was the last structural change, added when the flat spec list exceeded 30 files.

| Date | Document Count | Folders |
|------|---------------|---------|
| December 2025 | ~20 | 5 (initial structure) |
| January 2026 | ~60 | 7 (added Archive, refined Development) |
| February 2026 | ~100 | 7 (stable, subcategories in Specs) |
| March 2026 | 140+ | 7 (stable) |

The structure supports at least 200-300 documents before needing reconsideration. Growth happens within folders, not by adding folders.

---

*Structure established during RaceLogPro development, December 2025 -- January 2026.*
*Stable for 2+ months with continuous document growth from 60 to 140+ files.*
