# UX Engineer Workflow Skills

Claude Code skills for chained planning workflows: research → PRD → tasks → Jira.

## planning-with-files

Manus-style file-based planning for complex multi-step tasks. Creates `task_plan.md`, `findings.md`, and `progress.md`, with hooks that survive `/clear` for automatic session recovery.

Forked from [OthmanAdi/planning-with-files](https://github.com/OthmanAdi/planning-with-files).

```
npx skills@latest add uxengineer-pizzi/uxengineer-workflow-skills/planning-with-files
```

## findings-to-prd

Synthesises planning files into a structured `prd.md`. Run after `planning-with-files` to turn raw research into a shippable spec, no questions asked.

```
npx skills@latest add uxengineer-pizzi/uxengineer-workflow-skills/findings-to-prd
```

## prd-to-tasks

Breaks `prd.md` into independently-grabbable vertical-slice tasks with acceptance criteria, dependencies, and suggested order. Output is a single `tasks.md` ready for `tasks-to-jira`.

```
npx skills@latest add uxengineer-pizzi/uxengineer-workflow-skills/prd-to-tasks
```

## tasks-to-jira

Creates Jira issues from `tasks.md` via the Atlassian MCP. Handles story points, issue types, epics, and native blocking links by creating issues in dependency order.

```
npx skills@latest add uxengineer-pizzi/uxengineer-workflow-skills/tasks-to-jira
```

## Licence

MIT
