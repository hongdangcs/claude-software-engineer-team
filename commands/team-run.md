Run the AI engineering team autonomously end-to-end until the project is complete.

## Usage

```
/team-run
```

Requires an existing `board/project.json` (created by `/team-start`).

---

## What this command does

1. Read `board/project.json` to verify a project exists and is not already completed.
2. Read all `board/tickets/*.json`, `board/prs/*.json`, `board/test-results/*.json`.
3. Read `config/model-assignments.json`.
4. Spawn the **orchestrator** agent with the full board state.
5. The orchestrator runs continuously, spawning sub-agents and cycling until
   `project.status === "completed"` or a halt condition is reached.

---

## Orchestrator payload

```json
{
  "project": "<full board/project.json>",
  "all_tickets": ["<all ticket objects>"],
  "config": "<full config/model-assignments.json>",
  "board_path": "<absolute path>/board",
  "codebase_root": "<project.codebase_root>",
  "invocation_context": {
    "cycle": 1,
    "triggered_by": "team-run",
    "timestamp": "<ISO timestamp>"
  }
}
```

---

## Halt conditions

The orchestrator stops when any of these is true:
- `project.status === "completed"` → success
- All tickets are `DONE` → success
- A stall is detected (same ticket stuck for 3+ cycles) → partial halt, report stalled tickets
- A merge conflict or integration failure occurs → halt and report

After halting, print a summary:
- Project name and final status
- Ticket statuses (count per status)
- Total cycles run
- Any stalled or failed tickets

---

## Human-in-the-loop mode

To pause after each orchestrator cycle and review before continuing, use
`/team-next` instead of `/team-run`. This is useful for:
- Debugging a stuck ticket
- Reviewing intermediate code before the reviewer sees it
- Manually editing a ticket's context before re-running
