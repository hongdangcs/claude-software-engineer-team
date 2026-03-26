# claude-team

An autonomous AI software engineering team that manages the full development lifecycle using Claude agents, Jira, and Confluence.

## Overview

`claude-team` in cli to start.

```
/team-start → [GATE 1: Review PRD] → /team-next → [GATE 2: Review Tickets] → /team-next → ... → [GATE 3: UAT] → /team-next → Release
```

## Agents

| Agent | Model | Role |
|---|---|---|
| `orchestrator` | Opus 4.6 | Surveys Jira board, dispatches agents each cycle |
| `requirements-analyst` | Opus 4.6 | Turns a plain-English description into a Confluence PRD + Jira Epic |
| `ticket-creator` | Sonnet 4.6 | Decomposes approved requirements into Jira tickets |
| `software-engineer` | Sonnet 4.6 (Opus for `complexity:high`) | Implements each ticket |
| `code-reviewer` | Sonnet 4.6 | Reviews implementation against acceptance criteria |
| `qa-engineer` | Haiku 4.5 | Writes and runs tests |
| `release-manager` | Sonnet 4.6 | Merges branches and cuts a release |

## Commands

| Command | Description |
|---|---|
| `/team-start` | Initialize a new project and kick off the pipeline |
| `/team-next` | Advance to the next stage (use after human gates) |
| `/team-run` | Run autonomously end-to-end until project complete |
| `/team-status` | Show current board state |

## Quickstart

1. Have an existing Jira project and Confluence space ready.
2. Run `/team-start` and supply:
   - Project name
   - Plain-English description of what to build
   - Jira project key (e.g. `TEAM`)
   - Confluence space key (e.g. `TEAM`)
   - Absolute path to the target codebase
3. Review the generated PRD in Confluence, then transition the Epic to `REQUIREMENTS_APPROVED`.
4. Run `/team-next` — the pipeline runs automatically through code → review → QA.
5. Perform UAT when prompted at Gate 3, then run `/team-next` to release.

## Human Gates

| Gate | Trigger | Action Required |
|---|---|---|
| Gate 1 | PRD generated | Review Confluence PRD → transition Epic to `REQUIREMENTS_APPROVED` |
| Gate 2 | Tickets created | Review Jira tickets → approve (→ `READY`) or reject (→ `BACKLOG` with feedback) |
| Gate 3 | Features pass QA | Manually test each `PENDING_UAT` ticket → approve or reject |

## Configuration

Model assignments and parallelism limits are in `config/model-assignments.json`.

```json
{
  "parallelism": {
    "max_concurrent_engineers": 3,
    "max_concurrent_reviewers": 5,
    "max_concurrent_qa": 5
  }
}
```

## Requirements

- Claude Code with Jira and Confluence MCP integrations configured
- An active Jira project and Confluence space
