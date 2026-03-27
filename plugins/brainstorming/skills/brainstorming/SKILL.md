---
name: brainstorming
description: Use when starting any creative or feature work with a fuzzy idea — before writing specs, building components, adding functionality, or modifying behavior. Explores intent, constraints, and design options through collaborative dialogue. Produces a structured design summary ready for /spec-writing.
---

# Brainstorming

## Overview

Turn a fuzzy idea into a clear, agreed-upon design intent through collaborative dialogue. The terminal output is a **design summary** — a structured artifact the user hands to `/spec-writing` to produce the full plan.

This skill ends when the user approves the design summary. It does **not** write spec documents, invoke spec-writing, or take any implementation action.

## HARD GATE

Do NOT write code, scaffold projects, or invoke any skill. This skill ends when it produces an approved design summary — nothing more.

---

## Process

Complete these steps in order:

1. **Explore project context** — read files, docs, recent commits
2. **Offer visual companion** — if visual questions ahead (own message only)
3. **Challenge the premise** — is this the right problem?
4. **Select mode** — EXPANSION, HOLD SCOPE, or REDUCTION
5. **Ask clarifying questions** — one at a time
6. **Propose 2-3 approaches** — with trade-offs and recommendation
7. **Present design** — section by section, get approval after each
8. **Output design summary** — structured hand-off block

---

## Step 1: Explore Project Context

Before asking anything, understand what already exists:
- Read relevant files, docs, CLAUDE.md, recent commits
- Identify existing patterns to build on vs. start fresh
- Assess scope: if the request spans multiple independent subsystems, flag it and help decompose into sub-projects before proceeding

---

## Step 2: Visual Companion Offer

When upcoming questions involve visual content, offer the companion in **its own message — no other content**:

> "Some of what we're working on might be easier to explain visually — mockups, diagrams, comparisons. Want me to show things in a browser as we go? (Requires opening a local URL)"

Even after acceptance, decide **per question** whether visual or text is clearer:
- **Browser:** mockups, wireframes, architecture diagrams, side-by-side comparisons
- **Text:** conceptual questions, tradeoff lists, requirement choices, scope decisions

---

## Step 3: Challenge the Premise

Before diving into solutions, challenge the framing. Ask yourself:

- Is this the right problem to solve? Is there a simpler framing?
- What existing code already partially solves this — extend vs. rebuild?
- What's the actual user/business outcome? Is this the most direct path?
- What would happen if we did nothing?

State your findings briefly. If a fundamentally better framing exists, surface it before asking anything else. If the plan looks right, say so and move on.

**Before moving to Step 4, explicitly record the following 6 fields.** If any are unknown, ask — one question at a time — until all are answered:
- **Current state:** What exists today, where does it stop?
- **Failure mode:** What breaks, in concrete terms?
- **Consequence:** What is the operational/user impact? (specific — "+500ms", "data loss", "broken UX" — not vague)
- **Audience:** Who is affected by this problem, and what decision does solving it enable for them?
- **Success criteria:** What observable behavior changes when this is solved correctly?
- **Surface:** UI, API, CLI, or backend-only?

---

## Step 4: Select Mode

Present the three modes and let the user choose. **Commit to the chosen mode — do not drift.**

**EXPANSION** — Dream big. Explore the ambitious version. What's the version that's 10x more useful for 2x the effort? Scope goes up. Build the cathedral.

**HOLD SCOPE** — The idea is right-sized. Understand it deeply and make it rigorous. No creep in either direction.

**REDUCTION** — Strip to essentials. What's the minimum that ships real value? Everything else is deferred.

**Defaults by context:**
- New feature or greenfield → EXPANSION
- Change to existing behavior → HOLD SCOPE
- "Keep it small" / "just make it work" / refactor → REDUCTION

Ask which mode before continuing.

---

## Step 5: Clarifying Questions

- **One question per message** — never multiple at once
- Prefer multiple-choice over open-ended
- Focus on: purpose, constraints, success criteria, out-of-scope
- **EXPANSION:** also ask what the ideal *experience* would feel like, not just what it does
- If scope seems too large for one spec, decompose first

---

## Step 6: Propose 2-3 Approaches

- Lead with your recommended option and explain why
- Include trade-offs for each
- **EXPANSION:** include the "platonic ideal" — what the best engineer with unlimited time and perfect taste would build; start from user experience, not architecture

---

## Step 7: Present Design

Present section by section. Ask "does this look right?" after each. Revise before moving on.

**Sections (scale each to its complexity):**
1. Intent and goals
2. Architecture and key components
3. Data flow and state
4. Edge cases and constraints
5. **EXPANSION only:** delight opportunities — 3+ adjacent <30-minute improvements that would make users think "oh, they thought of that"

---

## Step 8: Output Design Summary

Once the user approves the design, output this fenced block:

~~~
## Design Summary

**Mode:** [EXPANSION / HOLD SCOPE / REDUCTION]

**Problem Context:**
- Current state: [what exists today, where it stops]
- Failure mode: [what breaks, in concrete terms]
- Consequence: [user/operational impact — be specific]
- Audience: [who uses this and what decision it enables]
- Success criteria: [what observable behavior changes if it works]
- Surface: [UI / API / CLI / backend-only]

**Intent:** [One sentence: what are we building and why]

**12-Month Vision:** [How this fits into the ideal system a year from now]

**Chosen Approach:** [Name + one-sentence rationale]

**Key Components:**
- [Component or area 1]
- [Component or area 2]

**Constraints:**
- [Technical, UX, or scope constraint]

**Resolved Decisions:**
- [Decision and rationale]

**Out of Scope:**
- [Explicit deferrals]

**Delight Opportunities:** *(EXPANSION only)*
- [Adjacent improvement 1 — effort estimate]
- [Adjacent improvement 2 — effort estimate]

**Open Questions for Spec:**
- [Anything deferred to spec-writing]
~~~

Then tell the user:
> "Design summary complete. Copy the block above and pass it to `/spec-writing` to build the full plan."

---

## Key Principles

- **Challenge premises first** — explore the problem before jumping to solutions
- **Commit to the mode** — don't drift after the user selects
- **One question at a time** — never overwhelm
- **YAGNI ruthlessly** (HOLD SCOPE / REDUCTION) — cut anything not serving the core outcome
- **Approval section by section** — not all at once at the end
- **Stop at the summary** — do not proceed to spec, plan, or any implementation
