---
name: to-tasks
description: >
  Break a PRD into structured, independently-grabbable tasks with acceptance
  criteria, design references, dependencies, and suggested order. Reads from
  prd.md. Use when the user says /to-tasks, "break this into tasks", or
  "create tasks from the PRD".
---

# to-tasks

Break `prd.md` into structured tasks using vertical slices. Each task cuts
through all layers end-to-end. Output is a single `tasks.md` file ready to
feed into `to-jira`.

## Step 1: Check inputs

Read `prd.md` from the project root.

If `prd.md` is missing, stop and output:

> `prd.md` not found. Run `/to-prd` first to generate the PRD, then come
> back to `/to-tasks`.

Do not proceed.

## Step 2: Ask for Figma link

Before drafting tasks, ask the user:

> Is there a Figma file for this PRD? If yes, paste the link and I'll
> reference it on any task that has a UI component. If no, reply "none".

Wait for the response before continuing.

## Step 3: Draft vertical slices

Break the PRD into tracer bullet tasks. Each task is a thin vertical slice
that cuts through ALL integration layers end-to-end — schema, API, UI,
tests — not a horizontal slice of one layer.

<slice-rules>
- Each slice delivers a narrow but complete path through every layer
- A completed slice is demoable or verifiable on its own
- Prefer many thin slices over few thick ones
- Acceptance criteria must come from prd.md — do not invent them
- If prd.md has no acceptance criteria for a slice, flag it as an open
  question rather than guessing
</slice-rules>

**Task types:**
- **HITL** (human-in-the-loop) — requires a decision, design review, or
  sign-off before it can be merged. Examples: architectural decisions,
  design reviews, scope calls.
- **AFK** (away from keyboard) — can be implemented and merged without
  human interaction. Prefer AFK over HITL where possible.

## Step 4: Present the breakdown for review

Show the proposed tasks as a numbered list. For each task:

- **Title** — short descriptive name
- **Type** — HITL or AFK
- **Blocked by** — which other tasks must complete first (by number)
- **Has UI** — yes or no (determines whether Figma link is included)

Then ask the user:

> Does the granularity feel right — too coarse, too fine, or about right?
> Are the dependency relationships correct?
> Should any tasks be merged or split?
> Are the HITL/AFK assignments correct?

Iterate until the user approves the breakdown.

## Step 5: Write tasks.md

Once approved, write `tasks.md` to the project root using the template
below. Create tasks in dependency order — blockers first.

---

## Task [N]: [Title]

**Type**: HITL / AFK
**Suggested order**: [N] of [total]
**Blocked by**: Task [N] — [title] / None — can start immediately

### What to build

[Concise description of this vertical slice. End-to-end behaviour, not
layer-by-layer implementation.]

### Acceptance criteria

[Pulled directly from prd.md. If none exist for this slice, write:]
⚠️ No acceptance criteria found in PRD for this task. Needs definition
before implementation.

### Design reference

[Figma link if task has a UI component. Omit this section entirely if no UI.]

---

[Repeat for each task]

## Open questions

[Anything that could not be resolved from prd.md — missing acceptance
criteria, unclear scope, unresolved dependencies.]

---

## Step 6: Confirm

After writing `tasks.md`, output:

> `tasks.md` written to project root with [N] tasks. Run `/to-jira` next to create Jira tickets?

Wait for user confirmation before running anything else.
