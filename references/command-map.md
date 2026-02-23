# jiradc Command Map

Use this file as the source of truth for translating natural-language Jira requests to `jiradc`.
The content is based on:

- `/Users/O000142/Projects/jiradc-cli/docs/COMMAND_REFERENCE.md`
- `/Users/O000142/Projects/jiradc-cli/jiradc_cli/main.py`

## Root Commands

| Intent | Command template | Required | Optional |
|---|---|---|---|
| Login with Jira URL and cookie flow | `jiradc login --base-url <URL>` | `--base-url` | `--cookie`, `--no-cookie-from-clipboard`, `--skip-verify` |
| Logout / clear local auth | `jiradc logout` | none | none |
| Show authenticated user profile | `jiradc whoami` | none | `--raw` |

## Project Commands

| Intent | Command template | Required | Optional |
|---|---|---|---|
| List visible projects | `jiradc project list` | none | `--raw` |
| List project components | `jiradc project components <PROJECT_ID_OR_KEY>` | `PROJECT_ID_OR_KEY` | `--raw` |
| List project versions | `jiradc project versions <PROJECT_ID_OR_KEY>` | `PROJECT_ID_OR_KEY` | `--expand`, `--raw` |

## Issue Commands

| Intent | Command template | Required | Optional |
|---|---|---|---|
| Search issues by JQL | `jiradc issue search --jql '<JQL>'` | `--jql` | `--max-results`, `--start-at`, `--fields`, `--raw` |
| Read one issue | `jiradc issue get <ISSUE_KEY>` | `ISSUE_KEY` | `--expand`, `--raw` |
| Create issue | `jiradc issue create --project <PROJECT> --summary '<SUMMARY>'` | `--project`, `--summary` | `--issue-type`, `--description`, `--assignee`, `--raw` |
| List create-metadata issue types for a project | `jiradc issue createmeta-types --project <PROJECT>` | `--project` | `--max-results`, `--start-at`, `--raw` |
| List create-metadata fields for project + issue type | `jiradc issue createmeta-fields --project <PROJECT> --issue-type-id <ID>` | `--project`, `--issue-type-id` | `--max-results`, `--start-at`, `--raw` |
| Show editable fields metadata for an issue | `jiradc issue editmeta <ISSUE_KEY>` | `ISSUE_KEY` | `--raw` |
| Edit issue fields | `jiradc issue edit <ISSUE_KEY> ...` | `ISSUE_KEY` + at least one update flag | `--summary`, `--description`, `--priority`, `--assignee`, `--clear-assignee`, `--labels`, `--notify-users/--no-notify-users` |
| List issue comments | `jiradc issue comments <ISSUE_KEY>` | `ISSUE_KEY` | `--raw` |
| Add comment to issue | `jiradc issue comment-add <ISSUE_KEY> --body '<TEXT>'` | `ISSUE_KEY`, `--body` | `--raw` |
| Get one comment | `jiradc issue comment-get <ISSUE_KEY> <COMMENT_ID>` | `ISSUE_KEY`, `COMMENT_ID` | `--expand`, `--raw` |
| Update one comment | `jiradc issue comment-update <ISSUE_KEY> <COMMENT_ID> --body '<TEXT>'` | `ISSUE_KEY`, `COMMENT_ID`, `--body` | `--expand`, `--raw` |
| Delete one comment | `jiradc issue comment-delete <ISSUE_KEY> <COMMENT_ID>` | `ISSUE_KEY`, `COMMENT_ID` | none |
| Upload one or more attachments | `jiradc issue attachment-add <ISSUE_KEY> --file <PATH> [--file <PATH2>]` | `ISSUE_KEY`, `--file` | `--raw` |
| List available transitions | `jiradc issue transitions <ISSUE_KEY>` | `ISSUE_KEY` | `--raw` |
| Perform transition | `jiradc issue transition <ISSUE_KEY> --id <TRANSITION_ID>` | `ISSUE_KEY`, `--id` | `--comment` |
| Assign issue | `jiradc issue assign <ISSUE_KEY> --username <USERNAME>` | `ISSUE_KEY`, `--username` | none |
| List watchers | `jiradc issue watchers <ISSUE_KEY>` | `ISSUE_KEY` | `--raw` |
| Add watcher | `jiradc issue watcher-add <ISSUE_KEY>` | `ISSUE_KEY` | `--username` |
| Remove watcher | `jiradc issue watcher-remove <ISSUE_KEY> --username <USERNAME>` | `ISSUE_KEY`, `--username` | none |
| Show issue votes | `jiradc issue votes <ISSUE_KEY>` | `ISSUE_KEY` | `--raw` |
| Add vote | `jiradc issue vote-add <ISSUE_KEY>` | `ISSUE_KEY` | `--raw` |
| Remove vote | `jiradc issue vote-remove <ISSUE_KEY>` | `ISSUE_KEY` | none |
| List issue worklogs | `jiradc issue worklogs <ISSUE_KEY>` | `ISSUE_KEY` | `--raw` |
| Get one worklog | `jiradc issue worklog-get <ISSUE_KEY> <WORKLOG_ID>` | `ISSUE_KEY`, `WORKLOG_ID` | `--raw` |
| Add worklog | `jiradc issue worklog-add <ISSUE_KEY> --time-spent '<DURATION>'` | `ISSUE_KEY`, `--time-spent` | `--comment`, `--started`, `--adjust-estimate`, `--new-estimate`, `--reduce-by`, `--raw` |
| Update worklog | `jiradc issue worklog-update <ISSUE_KEY> <WORKLOG_ID> ...` | `ISSUE_KEY`, `WORKLOG_ID` + one update field | `--time-spent`, `--comment`, `--started`, `--adjust-estimate`, `--new-estimate`, `--raw` |
| Delete worklog | `jiradc issue worklog-delete <ISSUE_KEY> <WORKLOG_ID>` | `ISSUE_KEY`, `WORKLOG_ID` | `--adjust-estimate`, `--new-estimate`, `--increase-by` |
| Get issue picker suggestions | `jiradc issue picker --query '<TEXT>'` | `--query` | `--current-project-id`, `--current-issue-key`, `--current-jql`, `--show-subtasks/--no-show-subtasks`, `--show-subtask-parent/--no-show-subtask-parent`, `--raw` |
| List available issue link types | `jiradc issue link-types` | none | `--raw` |
| Create issue link | `jiradc issue link-create --type '<TYPE>' --inward-issue <ISSUE> --outward-issue <ISSUE>` | `--type`, `--inward-issue`, `--outward-issue` | `--comment` |

## Filter Commands

| Intent | Command template | Required | Optional |
|---|---|---|---|
| List favourite filters | `jiradc filter favourites` | none | `--expand`, `--raw` |
| Get filter by id | `jiradc filter get <FILTER_ID>` | `FILTER_ID` | `--expand`, `--raw` |
| Create filter | `jiradc filter create --name '<NAME>' --jql '<JQL>'` | `--name`, `--jql` | `--description`, `--favourite/--no-favourite`, `--raw` |
| Update filter | `jiradc filter update <FILTER_ID>` | `FILTER_ID` + at least one update flag | `--name`, `--jql`, `--description`, `--raw` |

## JQL Commands

| Intent | Command template | Required | Optional |
|---|---|---|---|
| Get JQL suggestions | `jiradc jql suggest` | none | `--field-name`, `--field-value`, `--predicate-name`, `--predicate-value`, `--raw` |

## Agile Commands

| Intent | Command template | Required | Optional |
|---|---|---|---|
| List boards | `jiradc agile board list` | none | `--max-results`, `--start-at`, `--name`, `--project`, `--type`, `--raw` |
| List board backlog issues | `jiradc agile board backlog <BOARD_ID>` | `BOARD_ID` | `--jql`, `--fields`, `--max-results`, `--start-at`, `--validate-query/--no-validate-query`, `--expand`, `--raw` |
| List board sprints | `jiradc agile board sprints <BOARD_ID>` | `BOARD_ID` | `--state`, `--max-results`, `--start-at`, `--raw` |
| List sprint issues | `jiradc agile sprint issues <SPRINT_ID>` | `SPRINT_ID` | `--jql`, `--fields`, `--max-results`, `--start-at`, `--validate-query/--no-validate-query`, `--expand`, `--raw` |
| Move issues to sprint | `jiradc agile sprint move-issues <SPRINT_ID> --issue <ISSUE> [--issue <ISSUE2>]` | `SPRINT_ID`, `--issue` | none |
| Rank issues | `jiradc agile issue rank --issue <ISSUE> [--issue <ISSUE2>] --before <ISSUE>` | `--issue` + exactly one of `--before`/`--after` | `--rank-custom-field-id`, `--raw` |
| Get board estimation for issue | `jiradc agile issue estimation <ISSUE_KEY> --board-id <BOARD_ID>` | `ISSUE_KEY`, `--board-id` | `--raw` |
| Set board estimation for issue | `jiradc agile issue estimation-set <ISSUE_KEY> --board-id <BOARD_ID> --value <VALUE>` | `ISSUE_KEY`, `--board-id`, `--value` | `--raw` |

## JQL Presets for Natural-Language Requests

- "my open issues" -> `assignee = currentUser() AND resolution = Unresolved ORDER BY updated DESC`
- "my in progress work" -> `assignee = currentUser() AND statusCategory = "In Progress" ORDER BY updated DESC`
- "project backlog for PROJ" -> `project = PROJ AND statusCategory != Done ORDER BY priority DESC, updated DESC`

## Two-Step Translation Patterns

### Transition by Name

When the user asks "move PROJ-123 to In Progress":

1. `jiradc issue transitions PROJ-123`
2. Find the transition id where name/to-state matches "In Progress".
3. `jiradc issue transition PROJ-123 --id <ID>`

### Search Then Act

When the user asks for actions on "my latest ticket":

1. Run a search command with an explicit order in JQL.
2. Resolve the issue key from the first result.
3. Run the requested action command using that key.

### Board Name Then Board Action

When the user asks for backlog/sprints using board name:

1. `jiradc agile board list --name '<BOARD_NAME>'`
2. Resolve board id from output.
3. Run board/sprint command with that id.

## Unsupported Intents

If an intent is not available in this command map (for example: admin schemes, user creation/deletion, project creation/deletion, board creation/deletion), state that it is not supported by current `jiradc` user-workflow surface and offer the closest supported command.
