# jiradc Source Behavior Notes

## Core assumptions
- API base path is built as `{base_url}/rest`.
- Authentication uses browser session cookie stored in local config.
- Config file path: `~/.config/jiradc-cli/config.json`.

## Authentication behavior
- `jiradc login` accepts either:
  - `--cookie`, or
  - clipboard value (`--cookie-from-clipboard` default).
- Verification sequence:
  1. `GET /api/2/myself`
  2. `GET /auth/1/session`
- Cookie verification tries minimized Jira-focused cookie first, then full cookie.

## Error behavior
- `ConfigError`: missing/invalid local config or auth setup.
- `JiraApiError`: HTTP status >= 400.
- `JiraTransportError`: network/requests failure.
- CLI commands route these through `_require_success(...)` and exit with readable errors.

## Output behavior
- Most commands support concise output plus optional `--raw`.
- `--raw` should be used when users request complete response payloads.
- Some endpoints may return non-JSON payloads; CLI falls back to raw text output.

## Adjustment semantics for worklogs
- `worklog-add` allows `--adjust-estimate` values: `new|leave|manual|auto`.
- `worklog-update` allows `--adjust-estimate` values: `new|leave|auto`.
- `worklog-delete` allows `--adjust-estimate` values: `new|leave|manual|auto`.
- Additional flags (`--new-estimate`, `--reduce-by`, `--increase-by`) are validated by mode.

## Agile constraints
- `agile sprint move-issues`: max 50 issues per request.
- `agile issue rank`: max 50 issues; requires exactly one of `--before` or `--after`.

## Known CLI gaps
- No automated tests in repo yet.
- No shared pagination abstraction yet.
- No typed response models (dict-based handling).
