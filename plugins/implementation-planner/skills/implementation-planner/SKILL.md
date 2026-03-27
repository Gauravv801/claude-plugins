---
name: implementation-planner
description: Use when you have a system architecture document and need a concrete, ordered task breakdown before coding begins. Use when the work spans multiple components, phases, or files and needs to be sequenced before implementation starts.
---

# Implementation Planner

## Overview

Convert a system architecture into a concrete, ordered implementation plan. The terminal output is an **implementation plan block** — a structured artifact the user works through task-by-task in focused coding sessions (one task at a time, exploring approaches and writing code).

This skill ends when the user approves the implementation plan. It does **not** write code, scaffold files, or begin any implementation.

## HARD GATE

Do NOT write code, create files, or begin implementation. This skill ends when it produces an approved implementation plan — nothing more.

---

## Process

Complete these steps in order:

1. **Read input artifacts** — architecture doc, spec, any flow analysis
2. **Explore existing codebase** — what already exists vs. what needs building
3. **Identify phases** — logical milestones that deliver incremental value
4. **Decompose into tasks** — one task = one focused coding session
5. **Order by dependency** — tasks within a phase, phases in sequence
6. **Review with user** — phase by phase, get approval before finalizing
7. **Output implementation plan** — structured hand-off block

---

## Step 1: Read Input Artifacts

Before doing anything else, read everything provided:
- System architecture document (from `/system-architect`)
- Spec or PRD (from `/spec-writing`)
- Flow analysis (from `/spec-flow-analyzer`) if available

If the architecture document is missing, ask for it. Do not generate a plan from memory.

---

## Step 2: Explore Existing Codebase

Use the Explore subagent to understand what already exists:
- Which components from the architecture are partially or fully built?
- What patterns, utilities, and abstractions exist that tasks should reuse?
- Where do new components slot in — new files, new modules, new routes?
- What dependencies or config changes are needed?

Planning to build something that already exists is waste. Surface this before planning.

---

## Step 3: Identify Phases

Group the work into 2–5 logical phases. Each phase should:
- Deliver something independently valuable or testable
- Represent a coherent milestone ("auth layer working", "data model complete", "UI connected")
- Be completable before the next phase begins

**Sizing check:** Fewer than 2 tasks per phase → merge. More than 8 tasks per phase → split.

---

## Step 4: Decompose Into Tasks

For each phase, break it into discrete tasks. Each task must be:
- **Scoped for one focused coding session** — a specific problem with a clear solution space, not a vague area
- **Independently verifiable** — has a clear done state
- **Specific about location** — names the files to create or modify

For each task, define:
- **Name** — verb phrase ("Add JWT middleware", "Create User schema")
- **Files** — exact paths to create or modify
- **What to build** — 2–4 sentences of specific change
- **Acceptance criteria** — observable, testable outcome
- **Complexity** — S (< 30 min) / M (30–90 min) / L (> 90 min)
- **Depends on** — task IDs that must complete first (e.g., Task 1.2)

**Anti-patterns — do not produce tasks like these:**
- "Set up the database" → too vague; split into schema, connection, migrations
- "Build the frontend" → entire phase, not a task
- Tasks with no acceptance criteria → untestable, not a real task

---

## Step 5: Order by Dependency

Within each phase, order tasks so:
- Foundation tasks (schemas, interfaces, config) come before dependent tasks
- Tasks with no dependencies are noted as parallelizable
- No task requires output from a later task in the same or later phase

---

## Step 6: Review With User

Present phase by phase. After each phase ask: "Does this look right before I move on?" Revise before proceeding.

Check:
- Are any tasks missing that the architecture implies?
- Are any tasks too large to be one focused coding session (exploring one problem and writing the solution)?
- Are the acceptance criteria actually testable?

---

## Step 7: Output Implementation Plan

Once the user approves all phases, output this fenced block:

~~~
## Implementation Plan

**Source:** [Architecture / spec this was derived from]
**Phases:** N  |  **Tasks:** N  |  **Size:** S / M / L / XL

---

### Phase 1: [Name] — [One-line goal]
**Goal:** [What this phase delivers]
**Depends on:** none / Phase N

#### Task 1.1: [Task Name]
- **Files:** `path/to/file.ts` (create) / `path/to/other.ts` (modify)
- **What to build:** [2–4 sentences]
- **Acceptance criteria:** [Observable, testable outcome]
- **Complexity:** S / M / L
- **Depends on:** none / Task N.N

#### Task 1.2: [Task Name]
...

---

### Phase 2: [Name] — [One-line goal]
...

---

**Parallelizable tasks:** [Tasks with no inter-dependencies]
**Deferred / out of scope:** [Architecture items not in this plan, and why]
~~~

Then tell the user:
> "Implementation plan ready. Work through tasks one at a time in dependency order, starting with Task 1.1. For each task: explore approaches, decide on a solution, then write the code."

---

## Key Principles

- **Read before planning** — never generate a plan from memory; always read the architecture first
- **Explore first** — don't plan to build what already exists
- **One task = one focused coding session** — one problem, one solution space; if it spans multiple concerns, split it
- **Acceptance criteria are mandatory** — a task without a testable done-state is not a task
- **Stop at the plan** — do not write code, do not implement, do not begin any coding work
