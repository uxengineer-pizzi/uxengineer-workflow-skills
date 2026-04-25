---
name: to-jira
description: >
  Create Jira issues from tasks.md using the Atlassian MCP. Handles story
  points, issue types, epics, and native blocking relationships. Use when
  the user says /to-jira, "create Jira tickets", or "push tasks to Jira".
---

# to-jira

Create Jira issues from `tasks.md` using the Atlassian MCP. Creates issues
in dependency order so blocking relationships can be set with real issue keys.

## Step 1: Check inputs

Read `tasks.md` from the project root.

If `tasks.md` is missing, stop and output:

> `tasks.md` not found. Run `/to-tasks` first to generate the task
> breakdown, then come back to `/to-jira`.

Do not proceed.

## Step 2: Confirm project key

Ask the user:

> What is the Jira project key for these tasks? (e.g. `TECH`, `XYCLE`)

Wait for the response before continuing.

## Step 3: Confirm issue type mapping

Ask the user:

> How should I map issue types?
>
> Suggestion based on your tasks:
> - **HITL** tasks → `Task` (requires human decision or review)
> - **AFK** tasks → `Story` (independently implementable)
>
> Reply "yes" to use this mapping, or tell me how you'd like to adjust it.

Wait for confirmation or adjustment before continuing.

## Step 4: Handle epics

Ask the user:

> Do any of these tasks belong to an epic?
>
> - If epics already exist in Jira, paste the epic issue key(s) and tell
>   me which tasks belong to each (e.g. "TECH-42 covers tasks 1–4").
> - If you'd like me to create a new epic first, tell me the epic name
>   and which tasks it should cover.
> - If no epic is needed, reply "none".

Wait for the response before continuing.

If the user wants a new epic created, create it via Atlassian MCP before
proceeding to issue creation. Note the created epic key for linking.

## Step 5: Estimate story points

For each task in `tasks.md`, estimate story points using this scale:

| Points | Meaning |
|--------|---------|
| 1 | Trivial — under an hour, no unknowns |
| 2 | Small — half a day, well understood |
| 3 | Medium — one day, some complexity |
| 5 | Large — two to three days, meaningful scope |
| 8 | Extra large — near a week, significant unknowns |

Base estimates on:
- Acceptance criteria count and complexity
- Whether the task has UI (add 1 point if Figma reference exists)
- Whether the task is blocked (blocked tasks are often more complex)
- HITL tasks tend toward 1–2 (decision/review, not implementation)

Present all estimates to the user before creating anything:

> Here are my story point estimates. Reply "ok" to confirm or adjust
> any individually (e.g. "task 3 should be 5").
>
> | # | Title | Type | Points | Reason |
> |---|-------|------|--------|--------|
> | 1 | [title] | AFK | 3 | Medium scope, one UI component |
> | 2 | [title] | HITL | 1 | Decision only, no implementation |
> ...

Wait for confirmation before creating any issues.

## Step 6: Create issues in dependency order

Create issues via Atlassian MCP starting with tasks that have no blockers,
then tasks whose blockers are now created. This ensures real issue keys are
available when setting blocking links.

For each issue, set:

- **Summary**: task title from `tasks.md`
- **Issue type**: Story or Task per the confirmed mapping
- **Story points**: confirmed estimate
- **Description**: "What to build" section from `tasks.md`, followed by
  acceptance criteria as a checklist
- **Epic link**: if applicable
- **Labels**: none — blocking relationships use native Jira links

After creating each issue, note its key (e.g. `TECH-123`) for use in
blocking links.

### Issue description format

```
[What to build content from tasks.md]

---

*Acceptance criteria*

- [ ] [criterion 1]
- [ ] [criterion 2]

*Design reference*

[Figma link if present, otherwise omit]
```

## Step 7: Set blocking relationships

After all issues are created, set native Jira "is blocked by" links for
any tasks that have blockers listed in `tasks.md`.

Use the Atlassian MCP link issue operation:
- Link type: `is blocked by`
- Inward: the blocked issue
- Outward: the blocking issue

## Step 8: Confirm

Once all issues and links are created, output a summary:

> Created [N] issues in [PROJECT-KEY]:
>
> | Key | Title | Type | Points | Blocked by |
> |-----|-------|------|--------|------------|
> | TECH-123 | [title] | Story | 3 | — |
> | TECH-124 | [title] | Task | 1 | TECH-123 |
> ...
>
> All blocking relationships have been set as native Jira links.

## Error handling

If the Atlassian MCP is unavailable, stop and output:

> Atlassian MCP is not connected. To enable it, add the Atlassian MCP
> server to your Claude Code configuration, then re-run `/to-jira`.
> Your `tasks.md` is ready and waiting — nothing needs to be redone.

If an individual issue creation fails, note the failure, continue with
remaining issues, and list all failures in the final summary so they can
be retried manually.
