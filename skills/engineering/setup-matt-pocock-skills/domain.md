# Domain Docs

How the engineering skills should consume this repo's domain documentation when exploring the codebase.

## Before exploring, read these

- **`CONTEXT.md`** at the repo root, or
- **`CONTEXT-MAP.md`** at the repo root if it exists — it points at one `CONTEXT.md` per context. Read each one relevant to the topic.
- **`docs/adr/`** — read ADRs that touch the area you're about to work in. In multi-context repos, also check `src/<context>/docs/adr/` for context-scoped decisions.

If any of these files don't exist, **proceed silently**. Don't flag their absence; don't suggest creating them upfront. The `/domain-modeling` skill (reached via `/grill-with-docs` and `/improve-codebase-architecture`) creates them lazily when terms or decisions actually get resolved.

## File structure

Single-context repo (most repos):

```
/
├── CONTEXT.md
├── docs/
│   ├── adr/
│   │   ├── 0001-event-sourced-orders.md
│   │   └── 0002-postgres-for-write-model.md
│   └── agents/
│       ├── architecture.md
│       ├── domain.md
│       ├── issue-tracker.md
│       ├── project-state.md
│       └── triage-labels.md
└── src/
```

Multi-context repo (presence of `CONTEXT-MAP.md` at the root):

```
/
├── CONTEXT-MAP.md
├── docs/adr/                          ← system-wide decisions
└── src/
    ├── ordering/
    │   ├── CONTEXT.md
    │   └── docs/adr/                  ← context-specific decisions
    └── billing/
        ├── CONTEXT.md
        └── docs/adr/
```

## Use the glossary's vocabulary

When your output names a domain concept (in an issue title, a refactor proposal, a hypothesis, a test name), use the term as defined in `CONTEXT.md`. Don't drift to synonyms the glossary explicitly avoids.

If the concept you need isn't in the glossary yet, that's a signal — either you're inventing language the project doesn't use (reconsider) or there's a real gap (note it for `/domain-modeling`).

## Flag ADR conflicts

If your output contradicts an existing ADR, surface it explicitly rather than silently overriding:

> _Contradicts ADR-0007 (event-sourced orders) — but worth reopening because…_

## Architecture doc

**`docs/agents/architecture.md`** describes the **current** system structure. It is the single source of truth for how the codebase is organized.

- **Read it** at the start of any `/implement` or `/improve-codebase-architecture` session to understand the current shape.
- **Rewrite it** (not edit in-place) when your work changes the structure: new modules, moved files, changed interfaces, removed components.
- **Never duplicate** what's in the code — describe structure and interfaces, don't paste code snippets.
- **No "deprecated" sections** — if code is removed, it's removed from the doc in the same commit.

## Project state doc

**`docs/agents/project-state.md`** tracks what's been built and what's next. Two sections with different rules:

### Frontier (rewrite every time)

The list of tickets that are unblocked and ready to work on. **Rewrite this section** after every ticket completion — don't append, replace entirely. This keeps it reflecting reality.

### Completed (append-only log)

Each completed ticket gets one entry. **Append only** — never edit or remove past entries. This is a historical log, not a living document.

Template for a completed entry:

```
### NN — Ticket title (YYYY-MM-DD)

- **Changed:** key files/modules affected
- **What works:** the behaviour this ticket delivered
- **Commit:** SHA
```

### When to update

The `/implement` skill updates `project-state.md` as part of its "done" step, before committing. If you're working outside `/implement`, update it manually when you finish a meaningful chunk of work.
