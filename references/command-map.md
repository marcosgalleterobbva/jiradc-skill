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

## Issue Commands

| Intent | Command template | Required | Optional |
|---|---|---|---|
| Search issues by JQL | `jiradc issue search --jql '<JQL>'` | `--jql` | `--max-results`, `--start-at`, `--fields`, `--raw` |
| Read one issue | `jiradc issue get <ISSUE_KEY>` | `ISSUE_KEY` | `--expand`, `--raw` |
| Create issue | `jiradc issue create --project <PROJECT> --summary '<SUMMARY>'` | `--project`, `--summary` | `--issue-type`, `--description`, `--assignee`, `--raw` |
| List issue comments | `jiradc issue comments <ISSUE_KEY>` | `ISSUE_KEY` | `--raw` |
| Add comment to issue | `jiradc issue comment-add <ISSUE_KEY> --body '<TEXT>'` | `ISSUE_KEY`, `--body` | `--raw` |
| List available transitions | `jiradc issue transitions <ISSUE_KEY>` | `ISSUE_KEY` | `--raw` |
| Perform transition | `jiradc issue transition <ISSUE_KEY> --id <TRANSITION_ID>` | `ISSUE_KEY`, `--id` | `--comment` |
| Assign issue | `jiradc issue assign <ISSUE_KEY> --username <USERNAME>` | `ISSUE_KEY`, `--username` | none |

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

## Unsupported Intents

If an intent is not available in this command map (for example: editing issue fields after creation, worklogs, board/sprint operations, admin configuration), state that it is not supported by current `jiradc` endpoints and offer the closest supported command.
