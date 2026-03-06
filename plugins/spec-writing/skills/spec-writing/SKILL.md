---
name: spec-writing
description: This skill should be used when the user asks to "write a spec", "write a PRD", "write a product spec", "document a feature", "draft a requirements doc", "turn this into a spec", or "write this up properly". Apply whenever the user wants a structured engineering or product specification — regardless of whether it involves a UI, an API, a background job, or a data pipeline.
---

# Spec Writing

Apply this skill whenever writing a product or engineering specification.

The goal of a spec is not to list what will be built. It is to make any reader — engineer, PM, or reviewer — immediately understand the problem, the consequences of not solving it, and exactly what correct looks like when it is solved.

Before writing anything, work through the pre-writing checklist. Then apply the six principles to every section you write.

---

## Pre-Writing Checklist

Answer these before writing the first line. If the user hasn't provided enough context, ask.

1. **What problem does this solve?**
   Not "what will be built" — what goes wrong today, in concrete terms, that this fixes?

2. **What currently exists?**
   What does the system already do? What does it check or validate? Where does it stop?

3. **What is the consequence of the gap?**
   What happens in production, for users, or to the business because the current system doesn't handle this? Be specific: latency, cost, broken UX, data loss, support tickets.

4. **Who uses this and what decision does it help them make?**
   The spec should be written for that person. Every section should answer a question they would actually ask.

5. **What does "correct" look like?**
   If the feature works perfectly, what observable behavior changes? Is there a measurable output? Can success be quantified?

6. **Does this have a user-facing surface?**
   UI, dashboard, CLI output, API response? Or is it a backend process with no direct surface?

---

## Six Principles — Apply to Every Section

### 1. Why before how

For every feature, check, or component: explain the consequence of not having it before explaining how it works.

If a section starts with "The algorithm works by..." without first explaining what goes wrong without it — rewrite it. The reader doesn't yet have a reason to care.

What a good "why" looks like:
- Names a specific failure mode ("the model splits its attention between both copies of the instruction")
- States the impact ("latency increases, inconsistent bot behavior")
- Is written in plain language, no jargon
- Does not use bullets — prose reads as more authoritative and complete

### 2. Concrete before abstract

Introduce any rule, threshold, or algorithm with a real example — using actual numbers — before stating the general form.

Wrong order:
> Target: at least 70% of the prompt should be static before the first variable appears.

Right order:
> If a prompt is 100 lines long and the first variable appears on line 10, only 10% of the prompt is ever cached. If the first variable appears on line 80, 80% is cached — 4× less recomputation per request. Target: at least 70% static.

The example makes the threshold feel discovered, not arbitrary.

### 3. Tangible consequences

Bad states must be described in operational or user terms, not just as flags.

Don't write: "A red score indicates poor cache efficiency."
Write: "A red score means +500ms or more added to every request."

Don't write: "Markdown syntax triggers a warning."
Write: "The TTS engine reads '**bold**' aloud as 'asterisk asterisk bold asterisk asterisk' — directly broken audio output, not a performance issue."

If you find yourself describing a problem without naming what the user or system experiences as a result, add it.

### 4. Measurable = threshold-able

Any output that can be measured gets explicit numeric boundaries. Three tiers is the default: Green (acceptable), Yellow (warning), Red (action required).

Format:
```
Thresholds:
- Green: [specific condition with number]
- Yellow: [specific condition with number]
- Red: [specific condition with number]
```

If an output cannot be measured (e.g., a qualitative LLM output), say so explicitly and describe what a human reviewer should look for instead. Never use vague language like "too many" or "excessively" where a number could be given.

### 5. User-perspective framing for product surfaces

When describing any UI, CLI output, dashboard, or API response — start with what the user experiences and what decision it enables, before listing fields or contracts.

Wrong:
> Data fields: `overall_score`, `rating`, `deploy_ready`, `file_path`

Right:
> The first thing the developer sees is a large overall score and a clear deploy readiness indicator: either "Ready to Deploy" or "Needs Work." This answers the single most important question — should I deploy this prompt? — without requiring them to read anything else. Below the score: the prompt filename, the change from the last analysis, and a sparkline of the last 5 scores.
>
> Data fields: `overall_score`, `rating`, `deploy_ready`, `file_path`, `score_delta`, `version_history`

### 6. Close with a real workflow

Every spec ends with a numbered, action-oriented walkthrough of how a real user uses the feature from start to finish. Written in second person. 6–10 steps.

This section serves two purposes:
- It validates that the spec is complete (if you can't write a coherent workflow, there are gaps)
- It gives the reader a mental model of the feature before they've read a line of implementation detail

If the feature is backend-only with no user interaction, describe the operator or developer workflow for deploying, monitoring, and debugging it.

---

## Deciding What Sections to Include

A spec is not a template to fill in. Decide which sections are needed based on what the feature actually is.

**Always include:**
- Problem framing (what goes wrong today, in concrete terms)
- Background (current state → gap → consequence → solution in one sentence)
- The feature or component descriptions, each with: why it matters + how it works + what correct looks like
- A closing workflow

**Include only if applicable:**
- **Thresholds / scoring** — only if outputs are measurable
- **UI / product surface sections** — only if there is a user-facing surface
- **Data model** — only if there are structured outputs that downstream code will consume
- **Benchmark / comparison data** — only if performance is being compared to a baseline or org-wide percentile
- **Algorithm detail** — only if the implementation is non-obvious and will be built from this doc

**Never include for completeness:**
Do not add a "UI section" to a backend spec just to have one. Do not add a threshold table to a qualitative check just to look rigorous. A section that doesn't serve the reader makes the spec worse.

---

## Signal Patterns by Section Type

### Problem framing
- 2–3 numbered questions that define the scope
- Each question is answerable with "yes/no/how well" — they become the evaluation criteria for the feature
- State immediately which questions this doc addresses vs. out of scope

### Background
- One narrative paragraph (not bullets): current state → gap → consequence → solution
- Ends with one sentence naming the solution and the question it answers

### Feature / component sections
- Opens with a rhetorical question in italics (*Does the prompt repeat itself in ways that waste tokens?*)
- "Why this matters" paragraph — consequence of not having this, in plain language
- Named sub-checks (a, b, c) where the feature has multiple dimensions
- Each sub-check: purpose sentence → algorithm as numbered steps → thresholds

### Algorithm descriptions
- Numbered steps, one action per step
- Specific enough to implement from
- Variables and computed values named consistently
- Edge cases called out explicitly (e.g., "if the prompt has only one variable, scatter_spread collapses to 0 — skip this metric")

### UI / product surface
- "What [the developer / user] sees:" paragraph first
- Then data fields table or list
- Thresholds repeated for reference

### Workflow
- Second person, numbered
- Describes the real decision path ("if failing → if passing → check X → deploy")
- Includes what to do when the feature surfaces a warning, not just the happy path

---

## What to Ask Before Writing

If the user gives a rough idea and the pre-writing checklist can't be answered from context:

Ask only what's missing. Do not ask about things that can be inferred. Do not ask about implementation language, team ownership, timelines, or anything not needed to write the spec.

The minimum you need: the problem it solves, who uses it, and what correct looks like.
