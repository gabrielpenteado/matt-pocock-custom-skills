# Issue tracker: Local Markdown

Issues and specs (you may know a spec as a PRD) for this repo live as markdown files in `.scratch/`.

## Conventions

- One feature per directory: `.scratch/<feature-slug>/`
- The spec is `.scratch/<feature-slug>/spec.md`
- Implementation issues are one file per ticket at `.scratch/<feature-slug>/issues/<NN>-<slug>.md`, numbered from `01` — never a single combined tickets file
- Triage state is recorded as a `Status:` line near the top of each issue file (see `triage-labels.md` for the role strings)
- Comments and conversation history append to the bottom of the file under a `## Comments` heading

## Commit ticket references

When a commit is associated with a ticket, end its message with the ticket reference on its own final line, using the ticket number from its filename (`.scratch/<feature-slug>/issues/10-<slug>.md` → ticket `10`):

```text
Ticket: 10
```

- **Never prefix the number with `#`.** GitHub links any `#NN` it sees, so a local ticket `#10` can silently point at an unrelated pull request or issue 10 on the remote. `Ticket: 10` has no native meaning anywhere, so nothing links to the wrong thing.
- **Exactly once, on the final line.** Don't repeat it in the subject or body. That keeps it searchable: `git log --grep='Ticket: 10'`.
- **Use the ticket's own number** — never substitute a PR number.
- **Unrelated maintenance commits** (dependency bumps, typo fixes, anything with no ticket) carry no reference.

## When a skill says "publish to the issue tracker"

Create a new file under `.scratch/<feature-slug>/` (creating the directory if needed).

## When a skill says "fetch the relevant ticket"

Read the file at the referenced path. The user will normally pass the path or the issue number directly.

## Wayfinding operations

Used by `/wayfinder`. The **map** is a file with one **child** file per ticket.

- **Map**: `.scratch/<effort>/map.md` — the Notes / Decisions-so-far / Fog body.
- **Child ticket**: `.scratch/<effort>/issues/NN-<slug>.md`, numbered from `01`, with the question in the body. A `Type:` line records the ticket type (`research`/`prototype`/`grilling`/`task`); a `Status:` line records `claimed`/`resolved`.
- **Blocking**: a `Blocked by: NN, NN` line near the top. A ticket is unblocked when every file it lists is `resolved`.
- **Frontier**: scan `.scratch/<effort>/issues/` for files that are open, unblocked, and unclaimed; first by number wins.
- **Claim**: set `Status: claimed` and save before any work.
- **Resolve**: append the answer under an `## Answer` heading, set `Status: resolved`, then append a context pointer (gist + link) to the map's Decisions-so-far in `map.md`.
