Advance the AI team workflow until the next human gate, then pause.

## Usage

```
/team-next
```

---

## What this command does

1. Read `config/model-assignments.json`.
2. Spawn the **orchestrator** agent with `triggered_by: "team-next"`.
3. The orchestrator loops automatically through all internal pipeline stages
   (engineer → code reviewer → QA engineer → re-work if needed) without pausing.
4. Stops only when a **human gate** is reached:
   - **Gate 2** — tickets awaiting your approval before engineers start
   - **Gate 3** — tickets that passed code review AND QA, awaiting your UAT sign-off
5. Prints the gate message and waits for your action in Jira.

You are never interrupted mid-pipeline (between engineer, reviewer, and QA steps).

---

## Human gates (the only two pause points)

| Gate | When | Your action |
|---|---|---|
| **Gate 2** | Ticket Creator just created tasks | Review each task on Jira board → add label `ready` to approve, `rejected` to reject |
| **Gate 3** | Code review ✓ + QA ✓ both passed | Checkout branch, test manually → add label `uat_approved` to approve, `rejected` to reject |

---

## Difference from `/team-run`

| | `/team-run` | `/team-next` |
|---|---|---|
| Stops at | Human gates | Human gates |
| After a gate | Waits for `/team-next` again | Waits for `/team-next` again |
| Best for | Same — use either | Same — use either |

Both commands behave identically now. `/team-run` is an alias.
