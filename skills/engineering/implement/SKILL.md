---
name: implement
description: "Implement a piece of work based on a spec or set of tickets."
disable-model-invocation: true
---

Implement the work described by the user in the spec or tickets.

Use /tdd where possible, at pre-agreed seams.

Run typechecking regularly, single test files regularly, and the full test suite once at the end.

Once done, use /code-review to review the work.

Then run `/update-project-state` to preserve project memory — it updates project-state.md, marks tickets done, and flags review needs.

Commit your work to the current branch. If the work belongs to a ticket, end the commit message with that ticket's reference on its own final line, so the commit can be found later with `git log --grep`. Read `docs/agents/issue-tracker.md` for the configured tracker and use the format its "Commit ticket references" section prescribes — don't assume one format for every tracker. Use the ticket's ID exactly as that tracker assigned it, never a PR/MR number. The reference appears exactly once, as that final line; do not repeat it in the subject or elsewhere in the message. Work with no ticket gets no reference.
