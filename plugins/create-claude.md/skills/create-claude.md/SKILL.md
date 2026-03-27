---
name: create-claude.md
description: Use when creating a CLAUDE.md file for a project from scratch, or when an existing CLAUDE.md is missing key sections like commands, architecture overview, gotchas, or DO NOTs.
---

# Create CLAUDE.md

A CLAUDE.md orients Claude to a project. Every section must earn its place — vague instructions produce no measurable change in output.

## When to Use
- User asks to "create a CLAUDE.md", "set up Claude docs", or "write project instructions for Claude"
- Starting a new project with no CLAUDE.md
- Existing CLAUDE.md is missing structure or full of vague rules

**Not for:** Updating a single section — just edit that section directly.

## Core Pattern: Three-Zone Structure

| Zone | Sections | Purpose |
|------|----------|---------|
| TOP | 1–3 | Orient Claude to the big picture fast |
| MIDDLE | 4–7 | Work safely and consistently |
| BOTTOM | 8–9 | Hard stops and pointers |

### TOP — Orient Claude immediately

**1. Project Description (1–3 lines)**
What + who + core stack. Nothing more.

**2. Key Commands**
List every script: dev, test, build, lint, deploy, migrations. Name the package manager explicitly.
```
Use npm, not pnpm or bun.
```

**3. Tech Stack + Architecture**
Stack list + top-level directory map (`/app — pages`, `/lib — utilities`).

### MIDDLE — Work safely

**4. Code Style and Conventions**
Measurable rules only. If a linter can enforce it, don't write it here.
- ✅ "TypeScript strict mode, no `any` types"
- ✅ "Named exports, not default exports"
- ❌ "Write clean code" — produces no measurable difference, wastes context
- ❌ "Always validate and sanitize input" — not actionable without specifics
- ❌ "Avoid X unless absolutely necessary" — vague threshold, not enforceable

Do NOT inline code snippets or schema examples. Reference the real files — inlined copies cause context rot.

**5. File/Folder Structure Map**
Covered by Architecture (#3) if concise. Add only what isn't obvious from directory names.

**6. Important Gotchas and Warnings**
Non-obvious constraints that cause breakage. Ask the user — these cannot be inferred from code alone.

**7. Git and Commit Conventions**
Branch naming, commit format, PR merge strategy.

### BOTTOM — Hard stops

**8. DO NOTs**
Specific and action-oriented. One rule per line. Placed last (recency effect).
- ✅ "Do not run `prisma db push` — always use `db:migrate`"
- ✅ "Do not create new `PrismaClient()` instances — import from `src/lib/db.ts`"
- ❌ "Don't do bad things" — not actionable
- ❌ "Don't commit secrets" — too generic, already obvious

Use emphasis sparingly. If everything is IMPORTANT, nothing is.

**9. References and @imports**
Point to deeper docs instead of inlining them:
```
See @docs/api-patterns.md for API conventions
See @docs/auth-flow.md for authentication details
See @.env.example for required environment variables
```

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Vague style rules ("write clean code") | Replace with measurable rule or delete |
| "Always validate input" without specifics | Name the validation library and where it applies, or delete |
| "Avoid X unless absolutely necessary/unavoidable" | Give a concrete threshold or delete (applies to any "unless" phrasing) |
| Inlining env var schema | Write `See @.env.example` instead |
| Inlining code/schema examples | Reference the real file instead |
| No package manager specified | Add "Use X, not Y or Z" to Commands |
| Emphasis on everything | Reserve bold/caps for 1–2 truly critical items |
| DO NOTs buried in the middle | Move to bottom zone — recency effect matters |
| Gotchas invented from code | Ask user for production constraints instead |
| Architecture as prose paragraphs | Use a table or directory map instead |
