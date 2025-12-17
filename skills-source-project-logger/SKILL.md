---
name: project-logger
description: Record project activity, decisions, and outputs; query history and status. Provides institutional memory across sessions.
triggers:
  - log this
  - record that
  - what happened with
  - project history
  - what's the status
  - מה הסטטוס
  - מה קרה עם הפרויקט
---

# Project Logger Skill

Maintain a running log of all project activities, decisions, and outputs. Serves as the project's institutional memory and provides queryable history.

## Purpose

This skill records all significant project activities, enabling context retention across sessions and providing a queryable history. It supports automatic logging after actions and manual queries for project status.

## Modes

### Write Mode
- **Trigger:** After any significant action
- **Purpose:** Record activities as they happen

### Read Mode
- **Trigger:** Status check, context loading
- **Returns:** Relevant log entries
- **Purpose:** Provide historical context

### Summarize Mode
- **Trigger:** Project overview request
- **Returns:** Condensed project history
- **Purpose:** Quick status understanding

## Log Entry Types

| Type | Description | Auto-Triggered By |
|------|-------------|-------------------|
| **PROJECT_CREATED** | New project brief created | project-brief skill |
| **TASKS_DEFINED** | Task list created or updated | task-engine skill |
| **TASK_COMPLETED** | Specific task marked done | task-engine / Taskmaster |
| **TASK_DELEGATED** | Task assigned to agent | delegation-advisor skill |
| **DECISION_MADE** | Human made a decision | checkpoint |
| **BLOCKER_IDENTIFIED** | Issue blocking progress | any skill / manual |
| **BLOCKER_RESOLVED** | Issue resolved | any skill / manual |
| **MILESTONE_REACHED** | Major milestone completed | task-engine skill |
| **OUTPUT_DELIVERED** | Deliverable produced | any skill |

## Output Format

```markdown
# Project Log: {project_name}

**Project Started:** {start_date}
**Last Updated:** {timestamp}

---

## {date}

### {timestamp} - {action_type}
**Actor:** {human|skill_name|external_agent}
**Action:** {what_was_done}
**Output:** {deliverable_or_result}
**Decision:** {if_decision_was_made}
**Next Steps:** {identified_follow_ups}
**Blockers:** {if_any}

---
```

See `resources/log_template.md` for full template.

## Workflow

### Write Mode Workflow

**Step 1: Receive Notification**

Receive action notification with:
- `skill_name`: Which skill performed the action
- `action_type`: One of the 9 LOG_ENTRY_TYPES
- `action_details`: What was done
- `outputs`: Any deliverables produced
- `next_steps`: Follow-up actions identified

**Step 2: Format Entry**

Structure into log entry format:
- Add timestamp
- Format as markdown section
- Include all relevant fields

**Step 3: Append to Log**

- Append to `{project}_log.md`
- Position: Top (newest first)
- Maintain chronological grouping by date

### Read Mode Workflow

**Step 1: Parse Query**

Understand what information is needed:

| Query Type | Example | Returns |
|------------|---------|---------|
| `recent` | "Last 5 entries" | Most recent N entries |
| `by_type` | "All decisions" | Entries of type DECISION_MADE |
| `by_date` | "Last week" | Entries from date range |
| `search` | "About budget" | Entries containing keyword |

**Step 2: Retrieve Entries**

Extract relevant entries from log file.

**Step 3: Return Formatted**

Return entries in readable format, grouped logically.

### Summarize Mode Workflow

**Step 1: Analyze Full Log**

Process entire log to extract:
- Key milestones reached
- Major decisions made
- Current blockers
- Recent activity

**Step 2: Generate Summary**

```markdown
## Project Status Summary: {project_name}

**Health:** {green|yellow|red}
**Progress:** {percent}% toward goals

### Recent Activity (Last 7 days)
- {activity_1}
- {activity_2}

### Key Decisions Made
- {decision_1}

### Current Blockers
- {blocker_1} (since {date})

### Next Milestones
- {milestone}: {target_date}
```

**Health Indicators:**
- 🟢 **Green**: Active progress, no blockers
- 🟡 **Yellow**: Slow progress or minor blockers
- 🔴 **Red**: Stalled, major blockers

## Example: Auto-Logging

**After project-brief creates a brief:**

```markdown
## 2025-12-17

### 18:30:00 - PROJECT_CREATED
**Actor:** project_brief skill
**Action:** Created new project brief
**Output:** ai_strategy_law_faculty_brief.md
**Decision:** N/A
**Next Steps:** Create task breakdown
**Blockers:** None
```

**After delegation-advisor assigns a task:**

```markdown
### 19:15:00 - TASK_DELEGATED
**Actor:** delegation_advisor skill
**Action:** Delegated research task
**Output:** Handoff prompt generated for Gemini
**Decision:** User approved Gemini for research
**Next Steps:** Execute research in Gemini
**Blockers:** None
```

## Example: Status Query

**User Request:**
> "מה הסטטוס של פרויקט AI Strategy?"

**Response:**

```markdown
## Project Status Summary: AI Strategy (Law Faculty)

**Health:** 🟡 Yellow
**Progress:** 25% toward goals

### Recent Activity (Last 7 days)
- Created project brief (Dec 17)
- Defined 5 initial tasks (Dec 17)
- Delegated research to Gemini (Dec 17)

### Key Decisions Made
- Chose to focus on governance framework first

### Current Blockers
- Waiting for research results (since Dec 17)

### Next Milestones
- Complete framework research: Dec 20
- Draft proposal for Dean: Dec 25
```

## Cross-Interface Notes

- **Claude AI**: Log entries created via skill, stored in Project Knowledge
- **Claude Code**: Log entries written to filesystem
- Both use same format for compatibility

