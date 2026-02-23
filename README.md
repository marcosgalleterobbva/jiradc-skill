# jiradc-skill Codex Skill

Codex skill for operating the `jiradc` Jira Data Center CLI from natural-language requests.

## What teammates need
1. Install CLI:
   - `pip install jiradc-cli` or `pip install -e /path/to/jiradc-cli`
2. Configure Jira session:
   - Run `jiradc login --base-url <JIRA_URL>`
   - Provide browser cookie header (`Cookie:` value) when prompted
3. Install this skill under local Codex skills directory as `jiradc-skill`.

## Recommended distribution model
- Keep this skill in its own git repo.
- Ask teammates to clone/copy it into `$CODEX_HOME/skills/jiradc-skill`.
- Release CLI independently on PyPI (`jiradc-cli` package).

This keeps:
- command behavior/versioning in the CLI package,
- natural-language routing and Codex-specific instructions in the skill repo.

## Keeping skill and CLI aligned
When commands change in `jiradc_cli/main.py`, refresh inventory:

```bash
python3 scripts/generate_command_inventory.py \
  --source <PATH_TO_JIRADC_MAIN> \
  --output references/generated-command-inventory.md
```

Validation guardrail:
- Validate flags against `references/generated-command-inventory.md` before suggesting commands.
- Prefer `--raw` only when users ask for full payloads.

## Validate setup
After install, from Codex ask:
- "Use jiradc-skill to list my open issues."
- "Use jiradc-skill to comment on PROJ-123."
- "Use jiradc-skill to list board sprints for board 12."

Skill should return mapped commands such as `jiradc issue ...`, `jiradc project ...`, and `jiradc agile ...`.
