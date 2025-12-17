---
name: Secretary System Build
overview: Build the Secretary System skill bundle in Claude Code with all architecture document requirements, then package for Claude AI deployment. Uses references/ structure per Anthropic best practices.
todos:
  - id: setup
    content: Create directory structure ~/skills/secretary/ with subdirs
    status: in_progress
  - id: skill-core
    content: Create SKILL.md with triggers, intents, ADD cycle, FEEDBACK_LOOP, references
    status: pending
    dependencies:
      - setup
  - id: ref-brief
    content: Create references/project-brief.md with brain dump transformation
    status: pending
    dependencies:
      - setup
  - id: ref-task
    content: Create references/task-engine.md with Taskmaster MCP
    status: pending
    dependencies:
      - setup
  - id: ref-delegation
    content: Create references/delegation-advisor.md with 6 AGENT_PROFILES
    status: pending
    dependencies:
      - setup
  - id: ref-logger
    content: Create references/project-logger.md with 9 LOG_ENTRY_TYPES
    status: pending
    dependencies:
      - setup
  - id: templates
    content: Create 3 templates in assets/
    status: pending
    dependencies:
      - setup
  - id: agents
    content: Create 5 Claude Code agent definitions in agents/
    status: pending
    dependencies:
      - skill-core
  - id: scripts
    content: Create init_project.py and sync_taskmaster.py
    status: pending
    dependencies:
      - templates
  - id: deploy
    content: Symlink agents to ~/.claude/agents/ and verify
    status: pending
    dependencies:
      - agents
  - id: test
    content: Test all components + E2E workflow
    status: pending
    dependencies:
      - deploy
      - ref-brief
      - ref-task
      - ref-delegation
      - ref-logger
  - id: package
    content: Create package_skill.py and generate secretary.skill
    status: pending
    dependencies:
      - test
---

# Secretary System Implementation Plan v2.2

## Summary

Building the Secretary System as defined in [secretary_system_architecture.md](secretary_system_architecture.md), using Anthropic's skill authoring best practices:

- **Structure**: `references/` pattern (progressive disclosure) instead of nested `sub-skills/`
- **Location**: `~/skills/secretary/` (dev) → packaged as `.skill` for Claude AI
- **Cross-interface**: Works as Skill (Claude AI) AND Agent (Claude Code)

## Architecture

```mermaid
flowchart TB
    subgraph skill [SKILL.md - Orchestrator]
        triggers["Triggers: מזכירה, secretary, PA"]
        intents["Intents: PROJECT_INIT, TASK_MANAGE, STATUS_CHECK, DELEGATION, BRAIN_DUMP"]
        add["ADD Cycle: ASSESS → CHECKPOINT → DECIDE → CHECKPOINT → DO → LOG"]
        feedback["FEEDBACK_LOOP: Sub-skill → Orchestrator"]
    end
    
    subgraph refs [references/]
        brief["project-brief.md"]
        task["task-engine.md"]
        deleg["delegation-advisor.md"]
        logger["project-logger.md"]
    end
    
    subgraph assets [assets/]
        t1["brief_template.md"]
        t2["tasks_template.md"]
        t3["log_template.md"]
    end
    
    subgraph agents [agents/ - Claude Code]
        a1["secretary-orchestrator.md"]
        a2["project-initiator.md"]
        a3["task-manager.md"]
        a4["delegation-advisor.md"]
        a5["project-logger.md"]
    end
    
    skill --> refs
    refs --> assets
    skill -.->|skills: secretary| agents
```

## File Structure

```
~/skills/secretary/
├── SKILL.md                    # Main orchestrator + FEEDBACK_LOOP
├── references/
│   ├── project-brief.md        # Brain dump → brief
│   ├── task-engine.md          # CRUD + Taskmaster MCP
│   ├── delegation-advisor.md   # 6 AGENT_PROFILES
│   └── project-logger.md       # 9 LOG_ENTRY_TYPES
├── assets/
│   ├── brief_template.md
│   ├── tasks_template.md
│   └── log_template.md
├── scripts/
│   ├── init_project.py
│   └── sync_taskmaster.py
└── agents/
    ├── secretary-orchestrator.md
    ├── project-initiator.md
    ├── task-manager.md
    ├── delegation-advisor.md
    └── project-logger.md
```

## Gaps Filled from Architecture

| Gap | Where | Content |

|-----|-------|---------|

| FEEDBACK_LOOP | SKILL.md | Sub-agent feedback aggregation after each action |

| LOG_ENTRY_TYPES | ref-logger | 9 types: PROJECT_CREATED, TASKS_DEFINED, TASK_COMPLETED, TASK_DELEGATED, DECISION_MADE, BLOCKER_IDENTIFIED, BLOCKER_RESOLVED, MILESTONE_REACHED, OUTPUT_DELIVERED |

| AGENT_PROFILES | ref-delegation | claude_code, gemini, chatgpt, human_omer, taskmaster, other_mcps (monday, drive, gmail) |

## Execution Plan

### Phase 0: Setup

Create directory structure (`~/skills/secretary/` with subdirs)

### Phase 1A: Core SKILL.md

1. YAML frontmatter with bilingual triggers
2. Intent recognition (5 intents)
3. ADD Cycle governance with checkpoints
4. **FEEDBACK_LOOP section** (from architecture)
5. References linking
6. Bilingual examples

### Phase 1B-C: References + Templates (PARALLEL)

- `project-brief.md`: Brain dump transformation flow
- `task-engine.md`: Taskmaster MCP integration
- `delegation-advisor.md`: **6 full AGENT_PROFILES**
- `project-logger.md`: **9 LOG_ENTRY_TYPES** + history queries
- 3 templates in `assets/`

### Phase 1D-E: Agents + Scripts

- 5 Claude Code agent definitions (mirror the skill)
- `init_project.py`: Project initialization
- `sync_taskmaster.py`: Living docs ↔ Taskmaster

### Phase 1F: Deploy to Claude Code

- Symlink agents to `~/.claude/agents/`
- Verify loading

### Phase 2: Testing

- Test each component + full E2E workflow

### Phase 3: Package for Claude AI

- Create `package_skill.py`
- Generate `secretary.skill` ZIP
- Upload and verify in Claude AI

## Key Implementation Details

### FEEDBACK_LOOP (add to SKILL.md)

```yaml
FEEDBACK_LOOP:
  description: |
    After each sub-skill completes, it returns feedback:
    - What worked well
    - What was unclear
    - Suggested improvements
  storage: "Accumulated in orchestrator context for session"
```

### LOG_ENTRY_TYPES (add to ref-logger)

```
PROJECT_CREATED | TASKS_DEFINED | TASK_COMPLETED
TASK_DELEGATED | DECISION_MADE | BLOCKER_IDENTIFIED
BLOCKER_RESOLVED | MILESTONE_REACHED | OUTPUT_DELIVERED
```

### AGENT_PROFILES (ensure in ref-delegation)

- claude_code: Building, coding, deployment
- gemini: Research, multi-doc analysis
- chatgpt: Creative writing, brainstorming
- human_omer: Strategic decisions, reviews
- taskmaster: Task CRUD via MCP
- other_mcps: monday, google_drive, gmail

## Stats

| Metric | Value |

|--------|-------|

| Total Tasks in Taskmaster | 37 |

| Max Parallel Execution | 8 tasks |

| Phases | 7 (0-3) |

| Core Files | 13 |

| Dependencies Optimized | Yes |