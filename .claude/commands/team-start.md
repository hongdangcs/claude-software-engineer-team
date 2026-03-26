Initialize a new AI-team project using Jira + Confluence and kick off the pipeline.

## Usage

```
/team-start
```

You will be prompted for:
1. **Project name** — short identifier (e.g. `todo-api`)
2. **Project description** — plain-English description of what to build
3. **Jira project key** — the key of an existing Jira project (e.g. `TEAM`)
4. **Confluence space key** — the key of the Confluence space for documentation (e.g. `TEAM`)
5. **Target codebase path** — absolute path to the directory where code will be written

---

## What this command does

1. Read `config/model-assignments.json`.
2. Use `getVisibleJiraProjects` to verify the Jira project exists.
3. Use `getConfluenceSpaces` to verify the Confluence space exists.
4. Spawn the **requirements-analyst** agent with the project inputs.

The requirements-analyst will:
- Create a Confluence PRD page
- Create a Jira Epic in status `REQUIREMENTS_PENDING_REVIEW`

After it completes, print the Epic ID and Confluence page URL, then show:

```
[GATE 1] Requirements are ready for your review.

  Confluence PRD: {url}
  Jira Epic: {epic_id}

  → Read the PRD page, add comments or edit it as needed.
  → When satisfied, transition the Jira Epic to "REQUIREMENTS_APPROVED".
  → Then run /team-next to continue.
```

---

## Requirements-analyst payload

```json
{
  "project_name": "{project_name}",
  "project_description": "{description}",
  "raw_requirements": "{description}",
  "jira_project_key": "{jira_project_key}",
  "confluence_space_key": "{confluence_space_key}",
  "codebase_root": "{target_codebase_path}",
  "invocation_context": {
    "cycle": 1,
    "triggered_by": "team-start",
    "timestamp": "{ISO timestamp}"
  }
}
```
