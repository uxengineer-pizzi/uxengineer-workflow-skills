---
name: pr-description
description: Use when creating pull requests to generate a concise PR description outlining committed code changes with issue overview, broken down by file
---

Generate a concise PR description outlining code changes with issue overview, broken down by file.

## Behavior

When invoked, Claude will:

1. Analyze the changes made in the current context
2. Generate a PR description using the project's GitHub PR template format (from `.github/PULL_REQUEST_TEMPLATE`)
3. Include all required sections: Description, Visual Changes, How to Test, and Code Author Checklist
4. Include Bug Fix Details section if this is a bug fix
5. **ALWAYS** Format the output as raw markdown for easy copy-pasting into a PR

## Instructions

When invoked, follow these steps:

1. **Extract JIRA ticket from branch name**: Use `git branch --show-current` to get the current branch name
   - Look for pattern `TECH-XXXX` (where XXXX is a 4-digit number) in the branch name
   - If found, construct the Jira URL: `https://minviro.atlassian.net/browse/TECH-XXXX`
   - If not found, leave placeholder comment for manual entry
2. **Get committed changes**: Use `git diff` to see changes between branches (e.g., `git diff test...HEAD` or `git diff main...HEAD`)
3. **Review commit history**: Use `git log` to see recent commits and understand the context
4. **Analyze the changes**: Understand what issue is being addressed and what modifications were made
5. **Generate PR description**: Create a markdown description matching the GitHub PR template with:
   - **Description**: Overview of what the PR accomplishes, why it's needed, and link to Jira ticket (auto-extracted from branch name)
   - **Bug Fix Details**: If this is a bug fix, uncomment and fill in the bug details section (Bug, Root Cause, Fix, Prevention)
   - **Visual Changes**: Describe UI/UX changes or note "N/A - No visual changes"
   - **How to Test**: Provide clear step-by-step testing instructions
   - **Code Author Checklist**: Include the standard checklist
   - Raw markdown format ready to copy-paste into PR

## Output Format

Use the following template structure from `.github/PULL_REQUEST_TEMPLATE`:

```markdown
## Description

[TECH-1234](https://minviro.atlassian.net/browse/TECH-1234)

[Brief overview of what this PR accomplishes and why]

<!-- ## Bug Fix Details -->

<!--
For bug fixes, remove the comment markers and fill in:

**Bug:** What was happening?
**Root Cause:** Why was it happening?
**Fix:** How did you solve it?
**Prevention:** How are you preventing it from happening again?
-->

## Visual Changes

<!-- For any change that impacts UI or UX. Add screenshots, Loom videos, or GIFs to help reviewers understand the visual impact -->

[Describe visual changes or note "N/A - No visual changes"]

## How to Test

<!-- Steps to test the changes, e.g.:
1. Navigate to the Projects page
2. Click on "Create New Project"
3. Verify the modal opens correctly
-->

1. [First step]
2. [Second step]
3. [Verification step]

## Code Author Checklist:

- [ ] I added tests that show my feature works or my fix is effective.
- [ ] I have run the E2E test locally and there is no regression to the happy paths.
```

## Example Git Commands

- `git branch --show-current` - Get the current branch name (to extract JIRA ticket like TECH-1234)
- `git diff test...HEAD` - See all changes from test branch to current HEAD
- `git log test..HEAD --oneline` - See commit history since branching from test
- `git diff test...HEAD --name-only` - See just the list of changed files

## JIRA Ticket Extraction

Branch names typically follow the pattern: `TECH-XXXX-description` where XXXX is a 4-digit number.

Example: Branch `TECH-1234-add-user-auth` → JIRA ticket: `[TECH-1234](https://minviro.atlassian.net/browse/TECH-1234)`
