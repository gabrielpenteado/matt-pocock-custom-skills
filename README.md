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

This fork closes that gap. After each ticket, the agent automatically:

- Rewrites the frontier in `project-state.md` (which tickets are now unblocked)
- Appends a completion log entry (what was built, which files changed, the commit SHA)
- Rewrites `architecture.md` if the system structure changed
- Marks the ticket as `[x]` done in the tracker

The result: project docs stay accurate without manual effort, and every new session starts with a correct picture of the project.

## What Changed

### 1. `implement/SKILL.md` — Modified

**Original:** 6 lines. Build code, run tests, `/code-review`, commit.

**Customized:** Added two new sections:

- **Before starting** — reads `docs/agents/domain.md` to understand the project's documentation rules
- **Done** — before committing, runs 5 steps:
  1. Updates `docs/agents/project-state.md` (rewrites frontier, appends to completed log)
  2. Rewrites `docs/agents/architecture.md` if the system structure changed
  3. Marks the ticket as `done` in the configured tracker (including `[x]` on all checkboxes for local files)
  4. Runs `/code-review`
  5. Commits

**Problem solved:** The original had no instructions to keep project docs in sync. Code gets built, but `project-state.md` and `architecture.md` go stale. Tickets get marked as done but their checkboxes stay empty.

---

### 2. `setup-matt-pocock-skills/SKILL.md` — Modified

**Original:** Creates `issue-tracker.md`, `domain.md`, and `triage-labels.md`. Does not create `architecture.md` or `project-state.md`.

**Customized:** Now also creates:

- `docs/agents/architecture.md` — initial placeholder for system structure
- `docs/agents/project-state.md` — initial placeholder for project progress

Step 3 (Confirm) and Step 4 (Write) updated to include these new files. Step 5 (Done) notes they'll be populated by `/grill-with-docs` and `/implement`.

**Problem solved:** Without these placeholders, `/implement` has nothing to update. The setup now creates the full doc structure that the engineering skills expect.

---

### 3. `setup-matt-pocock-skills/domain.md` — Modified

**Original:** File structure shows only `CONTEXT.md`, `docs/adr/`, and `src/`. No mention of `docs/agents/`. No rules for maintaining architecture or project state docs.

**Customized:**

- Added `docs/agents/` to the file structure diagram (with `architecture.md`, `domain.md`, `issue-tracker.md`, `project-state.md`, `triage-labels.md`)
- Added **"Architecture doc"** section — rules for reading and rewriting `architecture.md`
- Added **"Project state doc"** section — rules for frontier (rewrite every time) and completed log (append-only)

**Problem solved:** The original `domain.md` didn't define how to maintain these docs. Without these rules, agents don't know when or how to update them.

---

### 4. `setup-matt-pocock-skills/architecture.md` — New File

Seed template for `docs/agents/architecture.md`. Includes sections for Overview, Structure, Data Flow, Key Interfaces, Dependencies, and Platforms.

**Problem solved:** Didn't exist in the original. Needed so `/setup-matt-pocock-skills` can create the file.

---

### 5. `setup-matt-pocock-skills/project-state.md` — New File

Seed template for `docs/agents/project-state.md`. Includes Frontier table (rewrite every time) and Completed log (append-only).

**Problem solved:** Didn't exist in the original. Needed so `/setup-matt-pocock-skills` can create the file.

---

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

Conflicts only occur if upstream changes `implement/SKILL.md` or `setup-matt-pocock-skills/` files.

---

## Reference

See the [original repository](https://github.com/mattpocock/skills) for the full list of skills and documentation.

---

## Credits

All original skills and methodology by [Matt Pocock](https://github.com/mattpocock). This fork adds automatic project documentation maintenance. Original repository: [mattpocock/skills](https://github.com/mattpocock/skills).
