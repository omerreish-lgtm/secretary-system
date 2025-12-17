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

## Log Entry Types Reference

| Type | Description | Triggered By |
|------|-------------|--------------|
| `PROJECT_CREATED` | New project brief created | project-brief skill |
| `TASKS_DEFINED` | Task list created or updated | task-engine skill |
| `TASK_COMPLETED` | Specific task marked done | task-engine / Taskmaster |
| `TASK_DELEGATED` | Task assigned to agent | delegation-advisor skill |
| `DECISION_MADE` | Human made a decision | checkpoint |
| `BLOCKER_IDENTIFIED` | Issue blocking progress | any skill / manual |
| `BLOCKER_RESOLVED` | Issue resolved | any skill / manual |
| `MILESTONE_REACHED` | Major milestone completed | task-engine skill |
| `OUTPUT_DELIVERED` | Deliverable produced | any skill |

---

## Project Health Indicators

- 🟢 **Green**: Active progress, no blockers
- 🟡 **Yellow**: Slow progress or minor blockers  
- 🔴 **Red**: Stalled, major blockers

---

*This log is maintained by the Secretary System.*
*Auto-updated after each significant action.*

