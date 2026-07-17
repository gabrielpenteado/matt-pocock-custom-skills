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

This fork closes that gap with a dedicated skill — `/update-project-state` — that preserves project memory after development work. It also ensures `architecture.md` is created during codebase exploration via `/grill-with-docs`.

## What Changed

### 1. `update-project-state/SKILL.md` — New Skill (Promoted)

A new engineering skill that maintains project memory after development work:

- **Updates `project-state.md`** — rewrites the Frontier section, appends to the Completed log (with SHA)
- **Marks tickets as done** — `[x]` on all checkboxes for local files, `done` label for GitHub/GitLab
- **Analyzes `architecture.md`** — flags if the system structure changed, suggests review
- **Analyzes `AGENTS.md`** — detects new permanent engineering rules, suggests addition
- **Analyzes `CONTEXT.md`** (single or multi-context) — detects new domain concepts, suggests `/grill-with-docs`
- **Analyzes ADRs** — detects decisions that need documentation, suggests `/grill-with-docs`
- **Generates a final report** — lists what was updated and what needs human review with concrete next actions

**Problem solved:** The original had no instructions to keep project docs in sync. Code gets built, but `project-state.md` and `architecture.md` go stale. Tickets get marked as done but their checkboxes stay empty.

---

### 2. `implement/SKILL.md` — Upstream + Chaining

Reverted to the original upstream. The only addition is a chaining line at the end:

> After committing, run `/update-project-state` to preserve project memory.

This follows the same pattern as `to-tickets` chaining into `/implement`.

---

### 3. `grill-with-docs/SKILL.md` — Modified

Added a section to create `docs/agents/architecture.md` when exploring the codebase. The file is created lazily — only when the session reveals the system structure. If it already exists, it is rewritten (not edited in-place) when the structure changes.

**Problem solved:** `architecture.md` describes system structure and only has value when filled with real understanding. Creating it during `/grill-with-docs` (which explores the codebase) is more meaningful than creating an empty placeholder during setup.

---

### 4. `setup-matt-pocock-skills/domain.md` — Modified

- Added `docs/agents/` to the file structure diagram
- Added **"Project state doc"** section with rules for Frontier (rewrite every time) and Completed (append-only)
- References `/update-project-state` for when updates happen

**Problem solved:** The original `domain.md` didn't define how to maintain project state docs. Without these rules, agents don't know when or how to update them.

---

### 5. `setup-matt-pocock-skills/project-state.md` — New File

Seed template for `docs/agents/project-state.md`. Includes Frontier table (rewrite every time) and Completed log (append-only).

**Problem solved:** Didn't exist in the original. Needed so `/setup-matt-pocock-skills` can create the initial file.

---

### 6. `audit-project-knowledge/SKILL.md` — New Skill (Promoted)

A periodic audit of the project's knowledge base. Not part of the normal dev loop — run monthly or after major milestones.

- **Audits AGENTS.md** — duplicate rules, contradictions, stale rules
- **Audits architecture.md** — outdated modules, stale interfaces, structural drift
- **Audits project-state.md** — frontier inconsistency, stale SHAs
- **Audits CONTEXT.md** (single or multi-context) — unused terms, duplicates, drifted definitions
- **Audits ADRs** — contradictory ADRs, reverted decisions, missing ADRs
- **Cross-references** between documents for contradictions
- **Generates a report** with severity levels (critical/warning/info) and specific skills to fix each problem

**Problem solved:** `/update-project-state` catches problems per-ticket, but issues accumulate over time. This skill finds the ones nobody noticed — contradictions between documents, knowledge drift, stale data.

---

## The Flow

```
/grill-with-docs  (creates CONTEXT.md, ADRs, architecture.md)
       |
   /to-spec
       |
  /to-tickets
       |
   /implement  (builds code, runs /tdd, /code-review, commits)
       |
/update-project-state  (updates project-state.md, marks tickets done, flags review needs)
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
