---
"mattpocock-skills": patch
---

Make commit ticket references follow the configured issue tracker instead of imposing `(#NN)` everywhere.

- **Local markdown tracker** now documents `Ticket: NN` as the final line of a ticket-associated commit, never `#NN` — GitHub auto-links `#NN`, so a local ticket number could point at an unrelated pull request. Still searchable with `git log --grep='Ticket: 10'`.
- **GitHub tracker** keeps native `(#123)` issue links, distinguishes the issue from the auto-assigned PR number, and no longer implies `Closes #123` on every commit (closing keywords have automatic side effects and should be used intentionally).
- **GitLab tracker** documents `(#123)` for issues versus `!123` for merge requests (numbered separately), with the same closing-keyword guidance.
- **`/implement`** reads `docs/agents/issue-tracker.md` and uses the format it prescribes, with the ticket's own ID, exactly once on the final line; unrelated commits carry no reference.

Docs pages for `setup-matt-pocock-skills` and `implement` re-synced.
