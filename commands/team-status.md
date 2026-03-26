Show the current state of the AI team project using live Jira + Confluence data.

## Usage

```
/team-status [epic_id]
```

If `epic_id` is not provided, list all Epics in the configured Jira project.

---

## What this command does

1. Use `getJiraIssue(epic_id)` to read the Epic (project summary).
2. Use `searchJiraIssuesUsingJql` to get all tickets:
   ```jql
   project = TEAM AND "Epic Link" = {epic_id} ORDER BY created ASC
   ```
3. Read the latest 5 comments on the Epic as the agent log.
4. Render the status report below.

---

## Output format

### Project Summary
```
Project: {epic summary}
Epic: {epic_id} | Status: {epic_status}
Confluence PRD: {link from Epic description}
```

### Pending Human Actions (shown prominently if any)

```
⚠  GATE 1 — Requirements Review
   Transition Epic {epic_id} to REQUIREMENTS_APPROVED when ready.

⚠  GATE 2 — Ticket Review ({N} tickets in PENDING_REVIEW)
   TEAM-2: Implement GET /todos endpoint
   TEAM-3: Implement POST /todos endpoint

⚠  GATE 3 — UAT ({N} tickets in PENDING_UAT)
   TEAM-2: Implement GET /todos endpoint  [branch: feature/TEAM-2-get-todos]
```

### Ticket Board

| Issue | Title | Status | Complexity |
|-------|-------|--------|------------|
| TEAM-2 | Implement GET /todos endpoint | ✅ DONE | medium |
| TEAM-3 | Implement POST /todos endpoint | 🔬 IN_TESTING | medium |
| TEAM-4 | Implement DELETE /todos/:id | 🔁 IN_PROGRESS | low |
| TEAM-5 | Add pagination to GET /todos | 👤 PENDING_REVIEW | low |

Status symbols:
- `[ ]` BACKLOG / PENDING_REVIEW
- `[~]` READY
- `[>]` IN_PROGRESS
- `[R]` IN_CODE_REVIEW
- `[T]` IN_TESTING
- `[U]` PENDING_UAT
- `[✓]` UAT_APPROVED
- `[M]` READY_TO_MERGE
- `[x]` DONE

### Recent Activity (last 5 Epic comments)
```
[2026-03-18 14:32] orchestrator: Cycle 3: dispatched 2 engineers (TEAM-3, TEAM-4), 1 reviewer (TEAM-2).
[2026-03-18 13:15] qa-engineer: QA PASSED on TEAM-2. Coverage: 87%. Moved to PENDING_UAT.
```

### Next Actions
Based on current statuses, describe what happens on the next `/team-next` cycle.
