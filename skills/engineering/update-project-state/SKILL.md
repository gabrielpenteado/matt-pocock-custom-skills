---
name: update-project-state
description: Preserve project memory after development work — update project-state.md, and flag whether architecture.md, AGENTS.md, CONTEXT.md, or ADRs need review.
disable-model-invocation: true
---

# Update Project State

Maintain the operational memory of the project after validated development work.

The main objective is ensuring that a future AI agent, developer, or different model can continue the project without reconstructing previous conversations.

## Core Principle

Responsibilities are separated:

- **`/grill-with-docs`** — understanding, discovery, domain knowledge, terminology, architectural decisions
- **`/implement`** — building changes
- **`/code-review`** — validating implementation quality
- **`/update-project-state`** — preserving project memory

## When To Run

Run after:

- `/implement` completes and commits
- completed features
- significant refactors
- before ending a long development session

## Process

### 1. Read project-state.md

Read `docs/agents/project-state.md`. If it does not exist, stop — run `/setup-matt-pocock-skills` first.

### 2. Update project-state.md

Analyze what was just built and update the file:

**Frontier** — rewrite this section entirely. List all tickets that are now unblocked (blockers are done). Remove tickets whose blockers are no longer satisfied.

**Completed** — append a new entry (never edit past entries):

```
### NN — Ticket title (YYYY-MM-DD)

- **Changed:** key files/modules affected
- **What works:** the behaviour this ticket delivered
- **Commit:** SHA
```

### 3. Mark the ticket as done

In the configured tracker:

- **Local files** (`.scratch/`): update the ticket's `Status:` field to `done` and mark all checkboxes as `[x]`
- **GitHub/GitLab**: apply the `done` label or close the issue per your convention

### 4. Analyze architecture.md

Read `docs/agents/architecture.md`. If it does not exist, skip this step.

Review whether recent changes affected:

- system structure
- modules
- dependencies
- data flow
- technical boundaries
- infrastructure

If the architecture changed significantly, add it to the "Review Needed" section of the final report. Do not rewrite automatically.

### 5. Analyze AGENTS.md

Read `AGENTS.md` at the repo root. If it does not exist, skip this step.

Review whether recent changes revealed a new permanent engineering rule.

**Requires review:**

- a new mandatory testing rule
- a new coding standard
- a permanent repository convention

**Does not require review:**

- a one-time implementation choice
- a local code pattern
- a temporary workaround

If needed, add it to the "Review Needed" section. Do not modify automatically.

### 6. Analyze CONTEXT.md

Do not modify CONTEXT.md.

Check whether changes introduced:

- new business concepts
- new terminology
- changed domain rules

If a `CONTEXT-MAP.md` exists at the root, check each context's `CONTEXT.md` that is relevant to the area changed.

If changes are detected, add it to the "Review Needed" section with the action: "Execute `/grill-with-docs` to resolve the new terms."

### 7. Analyze ADRs

Never create, modify, or delete ADRs.

If a significant architectural decision was discovered during the work — one that is hard to reverse, surprising without context, and the result of a real trade-off — add it to the "Review Needed" section with the action: "Execute `/grill-with-docs` to document the decision."

### 8. Final Report

After analysis, produce a summary:

```markdown
## Updated

- `docs/agents/project-state.md` — Frontier rewritten, completed log appended
- Ticket marked as done in [tracker]

## Review Needed

[Only include items that actually need review — omit this section entirely if nothing was flagged]

### architecture.md
- [what changed and why it needs review]
- **Action:** Run `/grill-with-docs` to rewrite, or edit manually

### AGENTS.md
- [what new rule was detected]
- **Action:** Add the rule manually to `AGENTS.md`

### CONTEXT.md
- [what new concept or term was introduced]
- **Action:** Run `/grill-with-docs` to resolve the term

### ADR
- [what decision needs documenting]
- **Action:** Run `/grill-with-docs` to create the ADR
```

## Rules

This skill must:

- preserve project continuity
- keep `project-state.md` accurate
- avoid duplicating knowledge
- suggest changes instead of making uncertain decisions

This skill must not:

- replace `/grill-with-docs`
- modify `CONTEXT.md`
- modify ADRs
- make architectural decisions
- rewrite `architecture.md` or `AGENTS.md` without explicit user confirmation
