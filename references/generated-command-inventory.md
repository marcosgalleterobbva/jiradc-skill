# jiradc CLI Command Inventory

Source: `jiradc_cli/main.py`

`*` marks required option/argument.

| Group | Command | Function | Parameters |
|---|---|---|---|
| agile board | `agile board backlog` | `agile_board_backlog` | `<board_id>*, --jql, --fields, --max-results, --start-at, --validate-query/--no-validate-query, --expand, --raw` |
| agile board | `agile board list` | `agile_board_list` | `--max-results, --start-at, --name, --project, --type, --raw` |
| agile board | `agile board sprints` | `agile_board_sprints` | `<board_id>*, --state, --max-results, --start-at, --raw` |
| agile issue | `agile issue estimation` | `agile_issue_estimation` | `<issue_key>*, --board-id*, --raw` |
| agile issue | `agile issue estimation-set` | `agile_issue_estimation_set` | `<issue_key>*, --board-id*, --value*, --raw` |
| agile issue | `agile issue rank` | `agile_issue_rank` | `--issue*, --before, --after, --rank-custom-field-id, --raw` |
| agile sprint | `agile sprint issues` | `agile_sprint_issues` | `<sprint_id>*, --jql, --fields, --max-results, --start-at, --validate-query/--no-validate-query, --expand, --raw` |
| agile sprint | `agile sprint move-issues` | `agile_sprint_move_issues` | `<sprint_id>*, --issue*` |
| filter | `filter create` | `filter_create` | `--name*, --jql*, --description, --favourite/--no-favourite, --raw` |
| filter | `filter favourites` | `filter_favourites` | `--expand, --raw` |
| filter | `filter get` | `filter_get` | `<filter_id>*, --expand, --raw` |
| filter | `filter update` | `filter_update` | `<filter_id>*, --name, --jql, --description, --raw` |
| issue | `issue assign` | `issue_assign` | `<issue_key>*, --username*` |
| issue | `issue attachment-add` | `issue_attachment_add` | `<issue_key>*, --file*, --raw` |
| issue | `issue comment-add` | `issue_comment_add` | `<issue_key>*, --body*, --raw` |
| issue | `issue comment-delete` | `issue_comment_delete` | `<issue_key>*, <comment_id>*` |
| issue | `issue comment-get` | `issue_comment_get` | `<issue_key>*, <comment_id>*, --expand, --raw` |
| issue | `issue comment-update` | `issue_comment_update` | `<issue_key>*, <comment_id>*, --body*, --expand, --raw` |
| issue | `issue comments` | `issue_comments` | `<issue_key>*, --raw` |
| issue | `issue create` | `issue_create` | `--project*, --summary*, --issue-type, --description, --assignee, --raw` |
| issue | `issue createmeta-fields` | `issue_create_meta_fields` | `--project*, --issue-type-id*, --max-results, --start-at, --raw` |
| issue | `issue createmeta-types` | `issue_create_meta_types` | `--project*, --max-results, --start-at, --raw` |
| issue | `issue edit` | `issue_edit` | `<issue_key>*, --summary, --description, --priority, --assignee, --clear-assignee, --labels, --notify-users/--no-notify-users` |
| issue | `issue editmeta` | `issue_editmeta` | `<issue_key>*, --raw` |
| issue | `issue get` | `issue_get` | `<issue_key>*, --expand, --raw` |
| issue | `issue link-create` | `issue_link_create` | `--type*, --inward-issue*, --outward-issue*, --comment` |
| issue | `issue link-types` | `issue_link_types` | `--raw` |
| issue | `issue picker` | `issue_picker` | `--query*, --current-project-id, --current-issue-key, --current-jql, --show-subtasks/--no-show-subtasks, --show-subtask-parent/--no-show-subtask-parent, --raw` |
| issue | `issue search` | `issue_search` | `--jql*, --max-results, --start-at, --fields, --raw` |
| issue | `issue transition` | `issue_transition` | `<issue_key>*, --id*, --comment` |
| issue | `issue transitions` | `issue_transitions` | `<issue_key>*, --raw` |
| issue | `issue vote-add` | `issue_vote_add` | `<issue_key>*, --raw` |
| issue | `issue vote-remove` | `issue_vote_remove` | `<issue_key>*` |
| issue | `issue votes` | `issue_votes` | `<issue_key>*, --raw` |
| issue | `issue watcher-add` | `issue_watcher_add` | `<issue_key>*, --username` |
| issue | `issue watcher-remove` | `issue_watcher_remove` | `<issue_key>*, --username*` |
| issue | `issue watchers` | `issue_watchers` | `<issue_key>*, --raw` |
| issue | `issue worklog-add` | `issue_worklog_add` | `<issue_key>*, --time-spent*, --comment, --started, --adjust-estimate, --new-estimate, --reduce-by, --raw` |
| issue | `issue worklog-delete` | `issue_worklog_delete` | `<issue_key>*, <worklog_id>*, --adjust-estimate, --new-estimate, --increase-by` |
| issue | `issue worklog-get` | `issue_worklog_get` | `<issue_key>*, <worklog_id>*, --raw` |
| issue | `issue worklog-update` | `issue_worklog_update` | `<issue_key>*, <worklog_id>*, --time-spent, --comment, --started, --adjust-estimate, --new-estimate, --raw` |
| issue | `issue worklogs` | `issue_worklogs` | `<issue_key>*, --raw` |
| jql | `jql suggest` | `jql_suggest` | `--field-name, --field-value, --predicate-name, --predicate-value, --raw` |
| project | `project components` | `project_components` | `<project_id_or_key>*, --raw` |
| project | `project list` | `project_list` | `--raw` |
| project | `project versions` | `project_versions` | `<project_id_or_key>*, --expand, --raw` |
| root | `login` | `login` | `--base-url*, --cookie, --cookie-from-clipboard/--no-cookie-from-clipboard, --skip-verify` |
| root | `logout` | `logout` | `` |
| root | `whoami` | `whoami` | `--raw` |
