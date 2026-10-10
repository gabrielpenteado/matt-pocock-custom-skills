<p>
  <a href="https://www.aihero.dev/s/skills-newsletter">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://res.cloudinary.com/total-typescript/image/upload/v1777382277/skills-repo-dark_2x.png">
      <source media="(prefers-color-scheme: light)" srcset="https://res.cloudinary.com/total-typescript/image/upload/v1777382277/skill-repo-light_2x.png">
      <img alt="Skills" src="https://res.cloudinary.com/total-typescript/image/upload/v1777382277/skill-repo-light_2x.png" width="369">
    </picture>
  </a>
</p>

# Skills For Real Engineers — Customized Fork

[![skills.sh](https://skills.sh/b/mattpocock/skills)](https://skills.sh/mattpocock/skills)

A fork of [Matt Pocock's skills](https://github.com/mattpocock/skills) with automatic project documentation maintenance.

## Why These Customizations Exist

The original skills are excellent for building code, but they have a gap: **project documentation goes stale**. When `/implement` finishes a ticket, it commits the code but never updates `project-state.md` or `architecture.md`. Over time, these docs diverge from reality — the frontier shows tickets that are already done, the architecture doesn't reflect new modules, and agents lose context.

This fork closes that gap with:

- **`/update-project-state`** — a dedicated skill that preserves project memory after development work
- **`/audit-project-knowledge`** — a periodic audit that catches contradictions, drift, and stale data across all project docs
- **Session start instructions** — every new agent session reads whatever of `project-state.md`, `architecture.md`, `CONTEXT.md`, and `issue-tracker.md` exists before doing anything else (`architecture.md` and `CONTEXT.md` are created lazily by `/grill-with-docs`, so a fresh session may not have them yet)

## What Changed

### 1. `update-project-state/SKILL.md` — New Skill (Promoted)

A new engineering skill that maintains project memory after development work:

- **Updates `project-state.md`** — rewrites the Frontier section, appends to the Completed log (with the ticket reference), and derives the Frontier from the tracker rather than session memory
- **Marks tickets as done** — `[x]` on all checkboxes for local files; closes the issue for GitHub/GitLab (no `done` label — closing is what clears native blockers)
- **Analyzes `architecture.md`** — flags if the system structure changed, suggests review
- **Analyzes `AGENTS.md`** — detects new permanent engineering rules, suggests addition
- **Analyzes `CONTEXT.md`** (single or multi-context) — detects new domain concepts, suggests `/grill-with-docs`
- **Analyzes ADRs** — detects decisions that need documentation, suggests `/grill-with-docs`
- **Generates a final report** — lists what was updated and what needs human review with concrete next actions

**Problem solved:** The original had no instructions to keep project docs in sync. Code gets built, but `project-state.md` and `architecture.md` go stale. Tickets get marked as done but their checkboxes stay empty.

---

### 2. `implement/SKILL.md` — Upstream + Chaining

Reverted to the original upstream. The only addition is a chaining line — but the order matters:

```
/code-review  →  /update-project-state  →  COMMIT
```

The commit happens **after** `/update-project-state`, so project-state updates, ticket marks, and review flags are all included in the same commit.

---

### 3. `grill-with-docs/SKILL.md` — Modified

Added a section to create `docs/agents/architecture.md` when exploring the codebase. The file is created lazily — only when the session reveals the system structure. If it already exists, it is rewritten (not edited in-place) when the structure changes.

**Problem solved:** `architecture.md` describes system structure and only has value when filled with real understanding. Creating it during `/grill-with-docs` (which explores the codebase) is more meaningful than creating an empty placeholder during setup.

---

### 4. `setup-matt-pocock-skills/SKILL.md` — Modified (Agent skills block)

The `## Agent skills` block template now includes a **Session start** section:

```markdown
### Session start

At the start of each session, read the files below that exist. Skip, without
comment, any that don't: `CONTEXT.md` and `architecture.md` are created lazily
by `/grill-with-docs`.
- `docs/agents/project-state.md` — what's been built, what's next, what's blocked
- `docs/agents/architecture.md` — how the codebase is structured (if it exists)
- `CONTEXT.md` (or each context's `CONTEXT.md` via `CONTEXT-MAP.md`) — domain glossary and vocabulary (if it exists)
- `docs/agents/issue-tracker.md` — where issues live and how to interact with them
```

**Problem solved:** Without this, a new agent session starts blind — it doesn't know what was built, what's pending, or how the project is structured. Now every session begins with full context.

---

### 5. `setup-matt-pocock-skills/domain.md` — Modified

- Added `docs/agents/` to the file structure diagram
- Added **"Project state doc"** section with rules for Frontier (rewrite every time) and Completed (append-only)
- References `/update-project-state` for when updates happen

**Problem solved:** The original `domain.md` didn't define how to maintain project state docs. Without these rules, agents don't know when or how to update them.

---

### 6. `setup-matt-pocock-skills/project-state.md` — New File

Seed template for `docs/agents/project-state.md`. Includes Frontier table (rewrite every time) and Completed log (append-only).

**Problem solved:** Didn't exist in the original. Needed so `/setup-matt-pocock-skills` can create the initial file.

---

### 7. `audit-project-knowledge/SKILL.md` — New Skill (Promoted)

A periodic audit of the project's knowledge base. Not part of the normal dev loop — run monthly or after major milestones.

- **Audits AGENTS.md** — duplicate rules, contradictions, stale rules
- **Audits architecture.md** — outdated modules, stale interfaces, structural drift
- **Audits project-state.md** — frontier inconsistency, stale ticket references
- **Audits CONTEXT.md** (single or multi-context) — unused terms, duplicates, drifted definitions
- **Audits ADRs** — contradictory ADRs, reverted decisions, missing ADRs
- **Cross-references** between documents for contradictions
- **Generates a report** with severity levels (critical/warning/info) and specific skills to fix each problem

**Problem solved:** `/update-project-state` catches problems per-ticket, but issues accumulate over time. This skill finds the ones nobody noticed — contradictions between documents, knowledge drift, stale data.

---

### 8. `prime/SKILL.md` — New Skill (Promoted)

An explicit, on-demand version of the **Session start** instructions from #4: reads `AGENTS.md`/`CLAUDE.md`, `CONTEXT.md` (or `CONTEXT-MAP.md`), and everything under `docs/agents/` (`project-state.md`, `architecture.md`, `domain.md`, `issue-tracker.md`), skims the codebase, then names the **frontier** — the next ticket ready to pick up, cross-checked against wherever tickets actually live in case `project-state.md` went stale — and stops.

- **Never implements** — reports the frontier ticket and confirms it's ready to start, nothing more
- **Verifies before trusting** — if `project-state.md` names a ticket that's already done or still blocked, it walks to the real frontier and flags the mismatch instead of repeating stale state
- **Works with no docs at all** — explores the repo directly and says so, instead of stalling

**Problem solved:** #4's `## Agent skills` block tells an agent to read those files at session start, but that's a passive instruction living in `AGENTS.md` — not every harness re-reads and acts on it the same way at the top of every session. `/prime` is the same job as an explicit command you can run yourself, on any harness, whenever you're picking a ticket-based project back up.

---

### 9. `to-github/SKILL.md` — New Skill (Promoted)

Migrates tickets off a **local-markdown** tracker onto GitHub Issues, wiring each ticket's `Blocked by` list as GitHub's **native issue dependencies** instead of prose, then repoints `docs/agents/issue-tracker.md` to GitHub via `/setup-matt-pocock-skills`. A **one-way door** — after it runs, GitHub is the tracker from then on, not just for the tickets migrated that run.

- **Reads the migration scope from `docs/agents/issue-tracker.md`** — per-feature (`.scratch/<feature-slug>/issues/`) or flat (`tickets/`) layout, whichever this repo uses
- **Skips what's already migrated** via a `GITHUB-IMPORT.md` manifest, so re-running only picks up what's new
- **Confirms with you before creating anything real** on GitHub

**Problem solved:** `setup-matt-pocock-skills` recommends local markdown for solo projects without a remote, but a project that grows past that point had no supported way to move its ticket history — and blocking edges — onto GitHub without losing them.

---

## The Flow

```
New session (explicit):
  /prime  (reads project-state.md + architecture.md + CONTEXT.md + issue-tracker.md,
           skims the codebase, names the frontier ticket, stops)
    — same job as the passive "Session start" instructions in AGENTS.md, on demand

Normal flow (per ticket):
/grill-with-docs  (creates CONTEXT.md, ADRs, architecture.md)
       |
   /to-spec
       |
  /to-tickets  ────────────────►  /to-github  (one-way: local tracker → GitHub Issues)
       |
   /implement
     1. /tdd
     2. /code-review
     3. /update-project-state  (updates docs, marks ticket, flags review needs)
     4. COMMIT                  (everything in one commit)

Periodic maintenance:
/audit-project-knowledge  (full knowledge base audit, monthly or after milestones)
```

## Installation

```bash
npx skills@latest add gabrielpenteado/matt-pocock-custom-skills
```

Then run `/setup-matt-pocock-skills` in your project to generate the docs in `docs/agents/`.

## Syncing with Upstream

```bash
git fetch upstream
git merge upstream/main
git push
```

Conflicts only occur if upstream changes `implement/SKILL.md`, `grill-with-docs/SKILL.md`, or `setup-matt-pocock-skills/` files.

---

## Reference

See the [original repository](https://github.com/mattpocock/skills) for the full list of skills and documentation.

---

## Credits

All original skills and methodology by [Matt Pocock](https://github.com/mattpocock). This fork adds automatic project documentation maintenance. Original repository: [mattpocock/skills](https://github.com/mattpocock/skills).
