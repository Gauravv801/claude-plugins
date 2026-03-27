---
name: spec-writing
description: Use when the user asks to "write a spec", "write a PRD", "write a product spec", "document a feature", "draft a requirements doc", "turn this into a spec", or "write this up properly" — for any structured engineering or product specification (UI, API, background job, or data pipeline).
---

# Spec Writing

## Overview
A spec's job is not to list what will be built — it's to make any reader immediately understand the problem, its consequences, and exactly what correct looks like when solved. This skill runs in two phases: **Phase 1** interrogates until no spec-blocking unknowns remain. **Phase 2** writes.

## When to Use
**Use when:** User asks to write a spec, PRD, requirements doc, or formal feature document.

## Phase 1: Interrogation

Before writing anything, reach complete, unambiguous understanding of what needs to be built.

**Step 1: Map what's already known.** If given a Design Summary from `/brainstorming`, map its Problem Context block directly to the pre-writing checklist — do not re-ask those fields. Add any missing or vague fields to the interrogation queue.

| Design Summary field | Checklist item |
|---|---|
| Failure mode | Problem |
| Current state | Current state |
| Consequence | Consequence |
| Audience | Reader |
| Success criteria | Success |
| Surface | Surface |

**Step 2: Interview relentlessly.** Interview the user relentlessly about every aspect of this plan using the askuserquestion tool until you reach a shared understanding. Walk down each branch of the design tree, resolving dependencies between decisions one by one.

**Step 3: Explore the codebase first.** Before asking the user any question, check if the answer exists in the codebase (contracts, data models, error handling, existing tests). If the code answers it, record the finding — don't ask the user.

**Exit:** When no unknowns remain, say: *"I have enough to write the spec. Key decisions resolved: [summary]. Beginning spec writing."*

---

## Phase 2: Spec Writing

## Pre-Writing Checklist
Answer before writing the first line.

1. **Problem** — What breaks today, concretely?
2. **Current state** — What does the system do, and where does it stop?
3. **Consequence** — Latency, cost, broken UX, data loss?
4. **Reader** — Who uses this? What decision does it enable?
5. **Success** — What observable behavior changes if it works?
6. **Surface** — UI, CLI, API, or backend-only?

## Six Principles

1. **Why before how** — Name the failure mode before explaining the mechanism. Any section opening with "the algorithm works by..." without first stating what breaks should be rewritten.

2. **Concrete before abstract** — Lead with a real example using actual numbers, then the general rule. The example makes thresholds feel discovered, not arbitrary.

3. **Tangible consequences** — Describe bad states as what users or systems experience: "+500ms per request," not "indicates poor efficiency."

4. **Measurable = threshold-able** — Measurable outputs get explicit Green/Yellow/Red numeric limits. Qualitative outputs: describe what a reviewer looks for instead.

5. **User-perspective framing** — For any UI, CLI, or API surface: what the user sees and what they decide, before fields or contracts.

6. **Close with a workflow** — End with a numbered second-person walkthrough (6–10 steps) covering warning paths. If you can't write it, there are gaps.

## Deciding What Sections to Include

**Always:** Problem framing, background (current state → gap → consequence → solution), feature/component descriptions, closing workflow.

**Only if applicable:** Thresholds, UI sections, data model, benchmark data, algorithm detail.

**Never for completeness** — sections that don't serve the reader make the spec worse.

## Quick Reference

| Section | Key pattern | Anti-pattern |
|---|---|---|
| Problem framing | Answerable questions → evaluation criteria | Vague goal with no failure mode |
| Background | Narrative: state → gap → consequence → solution | Bullets with no causal chain |
| Feature/component | Rhetorical question → why before mechanics | Algorithm first, no "what breaks" |
| Algorithm | Numbered steps, edge cases explicit | Prose with no step boundaries |
| UI/product surface | "What user sees" first, then fields | Fields before user experience |
| Workflow | Second person; includes warning path | Happy-path only |

## Common Mistakes

- **How before why** → Add "why this matters" paragraph before any algorithm.
- **Abstract problem** ("improve performance") → Name the specific failure mode and impact.
- **Vague thresholds** ("too slow") → Replace with numeric Green/Yellow/Red.
- **UI lists fields first** → Lead with what the user sees and what they decide.
- **No closing workflow** → Add it; can't write it? Find the gaps first.
- **Inapplicable sections** (threshold table for qualitative output) → Remove them.
