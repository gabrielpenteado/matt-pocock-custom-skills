---
name: implement
description: "Implement a piece of work based on a spec or set of tickets."
disable-model-invocation: true
---

# Implement

Implement the work described by the user in the spec or tickets.

## Before starting

Read `docs/agents/domain.md` — it defines how to consume and maintain project docs (`CONTEXT.md`, `architecture.md`, `project-state.md`, ADRs). Follow its rules throughout.

## During

Use /tdd where possible, at pre-agreed seams.

Run typechecking regularly, single test files regularly, and the full test suite once at the end.

## Done

Before committing, complete these steps in order:

1. **Update `docs/agents/project-state.md`:**
   - Rewrite the **Frontier** section — list all tickets that are now unblocked (blockers are done). Remove tickets whose blockers are no longer satisfied.
   - **Append** a new entry to the **Completed** log (never edit past entries):
     ```
     ### NN — Ticket title (YYYY-MM-DD)
     - **Changed:** key files/modules affected
     - **What works:** the behaviour this ticket delivered
     - **Commit:** SHA
     ```

2. **Rewrite `docs/agents/architecture.md`** (not edit in-place) if the work changed the system structure — new modules, moved files, changed interfaces, removed components. Skip this if the change was internal to an existing module.

3. **Mark the ticket as done** in the configured tracker:
   - **Local files** (`.scratch/`): update the ticket's `Status:` field to `done` and mark all checkboxes as `[x]`.
   - **GitHub/GitLab**: apply the `done` label or close the issue per your convention.

4. Use /code-review to review the work.

5. Commit your work to the current branch.
