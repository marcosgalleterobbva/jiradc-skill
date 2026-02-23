---
name: jiradc-skill
description: Natural-language translation layer for Jira Data Center CLI (`jiradc`). Use when users ask to authenticate, identify the current user, list projects/components/versions, search/get/create/edit issues, read/write comments, upload attachments, manage watchers/votes/worklogs, list/apply transitions, assign issues, manage filters, get JQL suggestions, run agile board/sprint/issue actions, or log out in Jira Data Center.
---

# JiraDC CLI Translator

Use this skill to translate Jira Data Center requests into exact `jiradc` commands for the user to run locally, then summarize command results when users paste output.

## Command Surface Source

- Primary local source: `/Users/O000142/Projects/jiradc-cli`
- Prefer reading `/Users/O000142/Projects/jiradc-cli/docs/COMMAND_REFERENCE.md` first.
- Confirm flags/arguments in `/Users/O000142/Projects/jiradc-cli/jiradc_cli/main.py` when needed.
- External reference (only when local repo is unavailable): `https://github.com/marcosgalleterobbva/jiradc-cli`

## Quick Start

1. Resolve executable:
   - Prefer `jiradc` if present in `PATH`.
   - Fallback to `python -m jiradc_cli`.
2. Map intent to command template using `references/command-map.md` and `references/nl-intent-playbook.md`.
3. Validate options against `references/generated-command-inventory.md`.
4. Never execute `jiradc` in Codex runtime. Always return commands for the user to run locally.

## Command Translation Rules

- Treat `jiradc login` as the entrypoint before authenticated commands.
- Treat issue keys as `<PROJECT>-<NUMBER>` (example: `PROJ-123`).
- Preserve user text exactly in quoted arguments such as `--summary`, `--description`, `--body`, `--comment`, and `--jql`.
- For destructive or state-changing actions (`issue create`, `issue edit`, `issue comment-add`, `issue comment-update`, `issue comment-delete`, `issue attachment-add`, `issue transition`, `issue assign`, watcher/vote/worklog mutations, `issue link-create`, filter create/update, agile ranking/move/estimation-set, `logout`), confirm intent before user execution unless the user explicitly asks to proceed.
- If the user asks for a transition by name (not ID), provide `jiradc issue transitions <ISSUE_KEY>` first, identify the matching transition ID from user-provided output, then provide `jiradc issue transition <ISSUE_KEY> --id <ID>`.
- If the request is outside the implemented command surface, state that clearly and offer the closest supported `jiradc` command.

## Routing And Validation

- Use `references/command-map.md` for grouped command templates.
- Use `references/nl-intent-playbook.md` for slot extraction and multi-step intent handling.
- Validate flags/options against `references/generated-command-inventory.md` before returning final commands.
- Use `references/source-behavior.md` for auth/session, cookie, endpoint, and error semantics.

If command details are uncertain or source changed, regenerate command inventory from source:

```bash
python scripts/generate_command_inventory.py \
  --source <PATH_TO_JIRADC_MAIN> \
  --output references/generated-command-inventory.md
```

## Response Format

When translating, respond with:

1. `Intent`: one short line.
2. `Command`: one fenced `bash` block.
3. `Missing data`: only when required fields are absent.

## Execution Guidance

- Never execute `jiradc` commands inside Codex runtime.
- Always provide exact command(s) for local execution plus the key fields to paste back.
- Prefer readable output and add `--raw` only when user asks for full JSON payloads.
- For mutating commands, include:
  - exact command,
  - expected effect,
  - one verification command where useful.

## Troubleshooting

Use `references/source-behavior.md` for auth/session, endpoint, and error semantics.

Codex-runtime connectivity limitation pattern:
- `JiraTransportError` / `Request failed` / DNS resolution failures (for example `NameResolutionError`).

When this happens:
1. Do not attempt to execute `jiradc` in Codex.
2. Provide exact `jiradc` command(s) for the user to run locally.
3. Continue once the user shares command output.

## Common Mappings

- "Who am I?" -> `jiradc whoami`
- "List projects" -> `jiradc project list`
- "Show project components for PROJ" -> `jiradc project components PROJ`
- "Show ticket PROJ-123" -> `jiradc issue get PROJ-123`
- "Find my open issues" -> `jiradc issue search --jql 'assignee = currentUser() AND resolution = Unresolved ORDER BY updated DESC'`
- "Comment on PROJ-123" -> `jiradc issue comment-add PROJ-123 --body '...'`
- "Log work on PROJ-123" -> `jiradc issue worklog-add PROJ-123 --time-spent '1h' --comment '...'`
- "List scrum boards" -> `jiradc agile board list --type scrum`
