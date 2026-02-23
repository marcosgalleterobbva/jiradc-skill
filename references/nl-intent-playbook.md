# jiradc Natural-Language Intent Playbook

Use this playbook to map user requests into concrete `jiradc` commands.

## Required Context
- Most issue actions require: `issue_key`
- Project actions require: `project_id_or_key`
- Filter updates require: `filter_id`
- Agile board/sprint actions require: `board_id` or `sprint_id`

## Intent Routing

| User intent pattern | Command template | Required fields |
|---|---|---|
| "who am I" | `jiradc whoami` | none |
| "list projects" | `jiradc project list` | none |
| "show versions for PROJ" | `jiradc project versions PROJ` | `project_id_or_key` |
| "find my open issues" | `jiradc issue search --jql 'assignee = currentUser() AND resolution = Unresolved ORDER BY updated DESC'` | none |
| "show PROJ-123" | `jiradc issue get PROJ-123` | `issue_key` |
| "create issue in PROJ" | `jiradc issue create --project PROJ --summary '<SUMMARY>'` | `project`, `summary` |
| "edit PROJ-123 summary" | `jiradc issue edit PROJ-123 --summary '<SUMMARY>'` | `issue_key`, update field |
| "comment on PROJ-123" | `jiradc issue comment-add PROJ-123 --body '<TEXT>'` | `issue_key`, `body` |
| "transition PROJ-123" | `jiradc issue transition PROJ-123 --id <TRANSITION_ID>` | `issue_key`, `transition_id` |
| "assign PROJ-123 to alice" | `jiradc issue assign PROJ-123 --username alice` | `issue_key`, `username` |
| "log work on PROJ-123" | `jiradc issue worklog-add PROJ-123 --time-spent '<DURATION>'` | `issue_key`, `time_spent` |
| "list favourite filters" | `jiradc filter favourites` | none |
| "suggest JQL values" | `jiradc jql suggest --field-name <FIELD> --field-value <VALUE_PREFIX>` | none |
| "list scrum boards" | `jiradc agile board list --type scrum` | none |
| "list sprints for board 12" | `jiradc agile board sprints 12` | `board_id` |
| "list issues in sprint 55" | `jiradc agile sprint issues 55` | `sprint_id` |

## Clarification Rules
- Ask for missing required fields in one concise question.
- If user requests a transition by name, resolve transition ID first.
- If user asks for board/sprint by name rather than ID, resolve ID first.
- For mutating actions, confirm intent unless user explicitly asks to proceed without extra confirmation.

## Codex Runtime Model
- Never execute `jiradc` commands in Codex runtime.
- Always provide exact command(s) for the user to run locally.
- Ask for pasted output (terminal text or JSON) and continue from that output.

## Safe Defaults
- Prefer read-only commands when mutation is not required.
- Prefer concise output; use `--raw` when user requests full JSON.
- Keep pagination bounded (`--max-results`, `--start-at`) for exploratory calls.
