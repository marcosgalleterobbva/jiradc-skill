---
name: jiradc-skill
description: Natural-language translation layer for Jira Data Center CLI (`jiradc`). Use when users ask to authenticate, identify the current user, list projects/components/versions, search/get/create/edit issues, read/write comments, upload attachments, manage watchers/votes/worklogs, list/apply transitions, assign issues, manage filters, get JQL suggestions, run agile board/sprint/issue actions, or log out in Jira Data Center.
---

# JiraDC CLI Translator

Translate Jira Data Center requests into exact `jiradc` CLI commands.

## Command Surface Source

- Primary local source: `/Users/O000142/Projects/jiradc-cli`
- Prefer reading `/Users/O000142/Projects/jiradc-cli/docs/COMMAND_REFERENCE.md` first.
- Confirm flags/arguments in `/Users/O000142/Projects/jiradc-cli/jiradc_cli/main.py` when needed.
- External reference (only when local repo is unavailable): `https://github.com/marcosgalleterobbva/jiradc-cli`

## Use This Workflow

1. Classify intent and map it to a command in `references/command-map.md`.
2. Extract all required arguments and options from the user request.
3. Ask only for missing required values.
4. Build one shell-ready command per user intent unless a multi-step flow is required.
5. Prefer readable output; add `--raw` only when the user asks for JSON/full payloads.
6. Execute commands when the user asks to run them; otherwise provide the command only.

## Command Translation Rules

- Treat `jiradc login` as the entrypoint before authenticated commands.
- Treat issue keys as `<PROJECT>-<NUMBER>` (example: `PROJ-123`).
- Preserve user text exactly in quoted arguments such as `--summary`, `--description`, `--body`, `--comment`, and `--jql`.
- For destructive or state-changing actions (`issue create`, `issue edit`, `issue comment-add`, `issue comment-update`, `issue comment-delete`, `issue attachment-add`, `issue transition`, `issue assign`, watcher/vote/worklog mutations, `issue link-create`, filter create/update, agile ranking/move/estimation-set, `logout`), confirm execution unless the user explicitly asks to run now.
- If the user asks for a transition by name (not ID), run `jiradc issue transitions <ISSUE_KEY>` first, find the matching transition ID, then run `jiradc issue transition <ISSUE_KEY> --id <ID>`.
- If the request is outside the implemented command surface, state that clearly and offer the closest supported `jiradc` command.

## Response Format

When translating, respond with:

1. `Intent`: one short line.
2. `Command`: one fenced `bash` block.
3. `Missing data`: only when required fields are absent.

## Quick Preset Mappings

- "Who am I?" -> `jiradc whoami`
- "List projects" -> `jiradc project list`
- "Show project components for PROJ" -> `jiradc project components PROJ`
- "Show ticket PROJ-123" -> `jiradc issue get PROJ-123`
- "Find my open issues" -> `jiradc issue search --jql 'assignee = currentUser() AND resolution = Unresolved ORDER BY updated DESC'`
- "Comment on PROJ-123" -> `jiradc issue comment-add PROJ-123 --body '...'`
- "Log work on PROJ-123" -> `jiradc issue worklog-add PROJ-123 --time-spent '1h' --comment '...'`
- "List scrum boards" -> `jiradc agile board list --type scrum`

Use `references/command-map.md` as the source of truth for command templates and required flags.
