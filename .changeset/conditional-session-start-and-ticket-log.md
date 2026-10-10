---
"mattpocock-skills": patch
---

Align the project-state flow with the real ticket lifecycle — conditional session reads, tracker as the source of truth, closing issues instead of labelling them done, and a ticket reference in the Completed log instead of a commit SHA that doesn't exist yet.

- **Session start is now conditional.** The `## Agent skills` block reads only the files that exist and skips the rest without comment, since `CONTEXT.md` and `architecture.md` are created lazily by `/grill-with-docs`. README's copy of the block is re-synced.
- **The tracker is the source of truth for ticket status.** `project-state.md` (seed and `domain.md`) now says the Frontier table is a cached summary; if the tracker and the table disagree, the tracker wins and `/prime` flags it. `/update-project-state` step 2 derives the Frontier from the tracker (open, unblocked, unassigned) instead of rebuilding it from session memory.
- **GitHub/GitLab closes the issue, no `done` label.** `/update-project-state` step 3 closes the issue per the tracker's convention (native blockers only clear when the blocker is closed, and `done` isn't in the triage vocabulary). The local tracker keeps `Status: done` + `[x]` checkboxes.
- **The Completed log records a ticket reference, not a commit SHA.** The template now uses `- **Ticket:** #NN` (local: `NN-slug`) in `/update-project-state` and `domain.md`. `/implement` ends its single commit message with the ticket reference (e.g. `(#12)`) so `git log --grep` finds it, and `/update-project-state` runs immediately before that commit rather than after it. README and `/audit-project-knowledge` references to stale SHAs are updated to stale ticket references.
