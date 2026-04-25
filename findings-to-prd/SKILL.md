---
name: to-prd
description: >
  Synthesise planning files into a structured PRD. Use when the user
  says /to-prd, "create a PRD", or "turn this into a PRD".
---

# to-prd

## Quick start

Run `/to-prd` after a planning session with planning-with-files. The skill
reads `findings.md`, `task_plan.md`, and `progress.md` from the project
root and writes a structured `prd.md`. No questions asked.

## Workflows

### 1. Read planning files

Read in this order. Each file has a specific role — do not treat them equally.

- [ ] Read `findings.md` first — **this is the primary source**. Research,
  discoveries, constraints, decisions made. The PRD is built from this.
- [ ] Read `task_plan.md` — supporting context only. Use phases to inform
  milestone structure, decisions to inform scope.
- [ ] Read `progress.md` — scan for technical risks and failed attempts only.
  Do not use session log entries as PRD content.
- [ ] If `findings.md` is missing, stop immediately and output:

  > `findings.md` not found. Run a planning session with planning-with-files first, then come back to `/to-prd`.

  Do not proceed. Do not attempt to synthesise from other files or context.
- [ ] If `task_plan.md` or `progress.md` are missing, note it and continue —
  they are supporting sources only.

### 2. Write prd.md

Write to `prd.md` in the project root using the team PRD template in
[TEMPLATE.md](TEMPLATE.md). Pull actual content from `findings.md` as the
primary source — do not leave placeholders where information exists. For
anything genuinely missing, make a reasonable assumption and log it in the
Open Questions section at the bottom.

- [ ] Fill metadata table (team, TL;DR, status, dates)
- [ ] Write problem statement from `findings.md`
- [ ] Define goal and success criteria from `findings.md`
- [ ] Describe high-level approach and alternatives from `findings.md`
- [ ] Write user stories per milestone — structure from `task_plan.md` phases,
  content from `findings.md`
- [ ] Fill scope table (in/out of scope) from `findings.md`
- [ ] List acceptance criteria from `findings.md`
- [ ] Surface security risks from `findings.md`
- [ ] Write implementation section — architecture and patterns from
  `findings.md`, technical risks from `progress.md`
- [ ] Add Open Questions section for anything assumed rather than found

### 3. Offer Notion publish

After writing `prd.md`, output this message exactly:

> `prd.md` written to project root.
>
> Would you like me to publish this to Notion?
> Reply **yes** to continue or **no** to stop here.

Wait for explicit confirmation. Do not proceed until the user replies.

### 4. Publish to Notion (only if confirmed)

- [ ] Ask for the target Notion page URL or database ID if not provided
- [ ] Use Notion MCP to create a new child page with the PRD content
- [ ] Confirm the created page URL to the user

If Notion MCP is unavailable:

> Notion MCP isn't connected. The contents of `prd.md` paste cleanly
> into Notion — headings and tables map directly to Notion blocks.

## Notes

See [TEMPLATE.md](TEMPLATE.md) for the full PRD template structure and
field definitions.

## Final step: Suggest next skill

After writing `prd.md`, output exactly:

> `prd.md` written. Run `/to-tasks` next to break it into tasks?

Wait for user confirmation before running anything else.
