# Secretary System Architecture
## Personal Assistant Agent Bundle for Project & Task Management
### Version 1.1 | December 2025

---

## Executive Summary

The Secretary System is a **skill bundle** designed to function as a personal assistant for managing projects, ventures, and initiatives that fall **outside** the formal academic scope. It operates alongside (not within) the existing Context Architecture v4 academic matrix.

### Core Principles

| Principle | Implementation |
|-----------|----------------|
| **Build Once, Use Everywhere** | Core skills built once in Claude Code, instantiated per project |
| **Cross-Interface Compatibility** | Works in both Claude AI (skill) and Claude Code (agent) |
| **Living Documents** | Todo lists and logs attached as knowledge files in relevant Projects |
| **ADD Governance** | Assess→Decide→Do cycle with human checkpoints |
| **Delegation Intelligence** | Smart routing to external agents (Claude Code, Gemini, MCPs) |
| **Continuous Learning** | Feedback loops from all sub-agents back to Orchestrator |

---

## Core vs Instance Architecture

The Secretary System follows a **Build Once, Instantiate Per Project** model:

```
╔═══════════════════════════════════════════════════════════════════════════════╗
║                    SECRETARY SYSTEM - LAYERED VIEW                            ║
╠═══════════════════════════════════════════════════════════════════════════════╣
║                                                                               ║
║  ┌─────────────────────────────────────────────────────────────────────────┐ ║
║  │                    CORE SECRETARY (Build Once)                          │ ║
║  │                                                                         │ ║
║  │  Skills:              Templates:           Logic:                       │ ║
║  │  • orchestrator       • brief_template     • ADD Cycle                  │ ║
║  │  • project_brief      • tasks_template     • Intent Recognition         │ ║
║  │  • task_engine        • log_template       • Delegation Rules           │ ║
║  │  • delegation_advisor                      • Agent Profiles             │ ║
║  │  • project_logger                                                       │ ║
║  │                                                                         │ ║
║  │  Location: /mnt/skills/user/secretary/                                  │ ║
║  │  Built in: Claude Code                                                  │ ║
║  │  Runs in: Claude AI (as Skill) OR Claude Code (as Agent)               │ ║
║  └─────────────────────────────────────────────────────────────────────────┘ ║
║                                    │                                          ║
║                                    │ instantiate per project                  ║
║                                    ▼                                          ║
║  ┌─────────────────────────────────────────────────────────────────────────┐ ║
║  │                 PROJECT INSTANCES (Per Project)                         │ ║
║  │                                                                         │ ║
║  │  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐         │ ║
║  │  │  AI Strategy    │  │   FoodWise      │  │   Project X     │         │ ║
║  │  │  (Law Faculty)  │  │                 │  │                 │         │ ║
║  │  │                 │  │                 │  │                 │         │ ║
║  │  │  • brief.md     │  │  • brief.md     │  │  • brief.md     │         │ ║
║  │  │  • tasks.md     │  │  • tasks.md     │  │  • tasks.md     │         │ ║
║  │  │  • log.md       │  │  • log.md       │  │  • log.md       │         │ ║
║  │  │  • context...   │  │  • context...   │  │  • context...   │         │ ║
║  │  │                 │  │                 │  │                 │         │ ║
║  │  └─────────────────┘  └─────────────────┘  └─────────────────┘         │ ║
║  │                                                                         │ ║
║  │  Location: Claude AI Project Knowledge Files                           │ ║
║  │            OR filesystem (when using Claude Code)                      │ ║
║  └─────────────────────────────────────────────────────────────────────────┘ ║
║                                                                               ║
╚═══════════════════════════════════════════════════════════════════════════════╝
```

### Analogy

| Concept | Analogy |
|---------|---------|
| Core Secretary | Operating System (macOS) - installed once |
| Project Instance | App with open document (Word + specific file) |
| Skills | System utilities - available everywhere |
| Living Documents | User files - specific to each project |

---

## Cross-Interface Compatibility

The Secretary System works in **both** Claude AI and Claude Code:

### Component Mapping

| Component | Claude AI | Claude Code | Framework Type | Primary Role |
|-----------|-----------|-------------|----------------|--------------|
| **secretary_orchestrator** | Skill (from /mnt/skills/) | Agent (orchestrates) | Skill Bundle Entry Point | Routing, ADD cycle, intent recognition |
| **project_brief** | Skill | Sub-agent / Tool | Skill | Create briefs from brain dumps |
| **task_engine** | Skill | Sub-agent + MCP calls | Skill + MCP Integration | Manage tasks, sync with Taskmaster |
| **delegation_advisor** | Skill | Sub-agent / Tool | Skill | Recommend assignments + generate prompts |
| **project_logger** | Skill | Sub-agent + File I/O | Skill + Filesystem | Record actions, read history |
| **Taskmaster** | MCP (via skill) | MCP (native) | External MCP | Task CRUD, dependencies |

### Behavior by Interface

| Action | In Claude AI | In Claude Code |
|--------|--------------|----------------|
| "מזכירה, פרויקט חדש" | Skill reads → interactive conversation → creates brief.md | Agent runs → asks questions → writes file |
| "מה הסטטוס?" | Skill reads from Project Knowledge files | Agent reads from filesystem |
| "תארגן את המשימות" | Skill → Taskmaster MCP | Agent → Taskmaster MCP (native) |
| "תעדכן את הלוג" | Skill guides, Claude AI writes | Agent writes directly |

---

## System Architecture Diagram

```
╔══════════════════════════════════════════════════════════════════════════════════╗
║                           SECRETARY SYSTEM ARCHITECTURE                          ║
╠══════════════════════════════════════════════════════════════════════════════════╣
║                                                                                  ║
║  ┌─────────────────────────────────────────────────────────────────────────┐    ║
║  │                         HUMAN LAYER (Omer)                              │    ║
║  │                                                                         │    ║
║  │   Input: Requests, ideas, updates, brain dumps                         │    ║
║  │   Output: Approvals, decisions, direction                              │    ║
║  └───────────────────────────────┬─────────────────────────────────────────┘    ║
║                                  │                                              ║
║                                  ▼                                              ║
║  ┌─────────────────────────────────────────────────────────────────────────┐    ║
║  │                    ORCHESTRATOR AGENT                                   │    ║
║  │                    ═══════════════════                                  │    ║
║  │                                                                         │    ║
║  │   SKILL: secretary_orchestrator                                        │    ║
║  │                                                                         │    ║
║  │   Responsibilities:                                                    │    ║
║  │   • Intent Recognition - parse user requests                           │    ║
║  │   • Route to appropriate sub-agent                                     │    ║
║  │   • Manage ADD Cycle (Assess→Decide→Do)                               │    ║
║  │   • Checkpoints - return control to Human                             │    ║
║  │   • Read from Log for context awareness                               │    ║
║  │   • Aggregate feedback from all sub-agents                            │    ║
║  │                                                                         │    ║
║  │   Triggers:                                                            │    ║
║  │   • "מזכירה" / "secretary" / "PA"                                      │    ║
║  │   • "new project" / "task" / "status" / "what's next"                 │    ║
║  │                                                                         │    ║
║  │   ◀══════════════════════════════════════════════════════════════╗    │    ║
║  │   ║              FEEDBACK LOOP (Knowledge Aggregation)           ║    │    ║
║  │   ╚══════════════════════════════════════════════════════════════╝    │    ║
║  │         ▲               ▲               ▲               ▲             │    ║
║  └─────────┼───────────────┼───────────────┼───────────────┼─────────────┘    ║
║            │               │               │               │                    ║
║            │ feedback      │ feedback      │ feedback      │ feedback          ║
║            │               │               │               │                    ║
║  ┌─────────┴───┐ ┌─────────┴───┐ ┌─────────┴───┐ ┌─────────┴───┐               ║
║  │  PROJECT    │ │    TASK     │ │ DELEGATION  │ │   LOGGER    │               ║
║  │  INITIATOR  │ │   MANAGER   │ │   ADVISOR   │ │             │               ║
║  │             │ │             │ │             │ │             │               ║
║  │ SKILL:      │ │ SKILL:      │ │ SKILL:      │ │ SKILL:      │               ║
║  │ project_    │ │ task_       │ │ delegation_ │ │ project_    │               ║
║  │ brief       │ │ engine      │ │ advisor     │ │ logger      │               ║
║  │             │ │             │ │             │ │             │               ║
║  │ Creates:    │ │ Creates:    │ │ Recommends: │ │ Records:    │               ║
║  │ • Scope     │ │ • Tasks     │ │ • Assignee  │ │ • Actions   │               ║
║  │ • Goals     │ │ • Subtasks  │ │ • Agent     │ │ • Outputs   │               ║
║  │ • Milestones│ │ • Priority  │ │ • Prompt    │ │ • Decisions │               ║
║  │ • Success   │ │ • Deps      │ │ • Handoff   │ │ • Timeline  │               ║
║  │   criteria  │ │ • Urgency   │ │   protocol  │ │             │               ║
║  └──────┬──────┘ └──────┬──────┘ └──────┬──────┘ └──────┬──────┘               ║
║         │               │               │               │                       ║
║         │               │               │               │                       ║
║         ▼               ▼               ▼               ▼                       ║
║  ┌─────────────────────────────────────────────────────────────────────────┐    ║
║  │                        LIVING DOCUMENTS                                 │    ║
║  │                        ════════════════                                 │    ║
║  │                                                                         │    ║
║  │   Storage: Markdown files attached as Project Knowledge in Claude AI   │    ║
║  │                                                                         │    ║
║  │   ┌──────────────────┐   ┌──────────────────┐   ┌──────────────────┐   │    ║
║  │   │  PROJECT BRIEFS  │   │    TODO LIST     │   │   RUNNING LOG    │   │    ║
║  │   │                  │   │                  │   │                  │   │    ║
║  │   │  {project}_      │   │  {project}_      │   │  {project}_      │   │    ║
║  │   │  brief.md        │   │  tasks.md        │   │  log.md          │   │    ║
║  │   │                  │   │                  │   │                  │   │    ║
║  │   │  • Project name  │   │  • Task list     │   │  • Timestamp     │   │    ║
║  │   │  • Scope         │   │  • Subtasks      │   │  • Action taken  │   │    ║
║  │   │  • Goals         │   │  • Priority      │   │  • By whom/what  │   │    ║
║  │   │  • Milestones    │   │  • Dependencies  │   │  • Output/result │   │    ║
║  │   │  • Deliverables  │   │  • Status        │   │  • Next steps    │   │    ║
║  │   │  • Success       │   │  • Assignee      │   │  • Blockers      │   │    ║
║  │   │    criteria      │   │    (Human/AI/    │   │  • Decisions     │   │    ║
║  │   │  • Constraints   │   │     Agent)       │   │    made          │   │    ║
║  │   └──────────────────┘   └────────┬─────────┘   └──────────────────┘   │    ║
║  │                                   │                                     │    ║
║  └───────────────────────────────────┼─────────────────────────────────────┘    ║
║                                      │                                          ║
║  ════════════════════════════════════╪══════════════════════════════════════   ║
║                                      │                                          ║
║                          ┌───────────┴───────────┐                              ║
║                          │    TASKMASTER MCP     │                              ║
║                          │                       │                              ║
║                          │  • Syncs with TODO    │                              ║
║                          │  • Task CRUD          │                              ║
║                          │  • Dependencies       │                              ║
║                          │  • Status tracking    │                              ║
║                          │  • PRD parsing        │                              ║
║                          └───────────┬───────────┘                              ║
║                                      │                                          ║
║  ════════════════════════════════════╪══════════════════════════════════════   ║
║                                      │                                          ║
║                                      ▼                                          ║
║  ┌─────────────────────────────────────────────────────────────────────────┐   ║
║  │                     EXTERNAL AGENTS / TOOLS                             │   ║
║  │                     ══════════════════════                              │   ║
║  │                                                                         │   ║
║  │   Delegation Targets (selected by Delegation Advisor):                 │   ║
║  │                                                                         │   ║
║  │   ┌─────────────┐   ┌─────────────┐   ┌─────────────┐   ┌───────────┐  │   ║
║  │   │ CLAUDE CODE │   │   GEMINI    │   │  CHATGPT    │   │   OTHER   │  │   ║
║  │   │             │   │             │   │             │   │   MCPs    │  │   ║
║  │   │ Best for:   │   │ Best for:   │   │ Best for:   │   │           │  │   ║
║  │   │ • Building  │   │ • Research  │   │ • Drafting  │   │ • Monday  │  │   ║
║  │   │ • Coding    │   │ • Deep dive │   │ • Creative  │   │ • Drive   │  │   ║
║  │   │ • Deploy    │   │ • Multi-doc │   │ • Review    │   │ • Gmail   │  │   ║
║  │   │ • Debug     │   │   analysis  │   │             │   │ • Calendar│  │   ║
║  │   └─────────────┘   └─────────────┘   └─────────────┘   └───────────┘  │   ║
║  │                                                                         │   ║
║  └─────────────────────────────────────────────────────────────────────────┘   ║
║                                                                                 ║
╚══════════════════════════════════════════════════════════════════════════════════╝
```

---

## ADD Cycle Integration

The Secretary System implements the ADD (Assess-Decide-Do) governance model with integrated logging:

```
┌────────────────────────────────────────────────────────────────────────────────┐
│                          ADD CYCLE WITH LOGGING                                │
│                                                                                │
│                                                                                │
│      ┌──────────────────────────────────────────────────────────────┐         │
│      │                                                              │         │
│      │    ┌─────────┐                                               │         │
│      │    │ ASSESS  │◀─────────────────────────────────────┐        │         │
│      │    │         │                                      │        │         │
│      │    │ • Read Log for context                        │        │         │
│      │    │ • Read Brief for scope                        │        │         │
│      │    │ • Read Tasks for status                       │        │         │
│      │    └────┬────┘                                      │        │         │
│      │         │                                           │        │         │
│      │         ▼                                           │        │         │
│      │    ┌─────────────────────────────────┐              │        │         │
│      │    │  CHECKPOINT: "Enough context?"  │              │        │         │
│      │    │  AI presents situation summary  │              │        │         │
│      │    └────┬────────────────────────────┘              │        │         │
│      │         │                                           │        │         │
│      │         ▼                                           │        │         │
│      │    ┌─────────┐                                      │        │         │
│      │    │ DECIDE  │                                      │        │         │
│      │    │         │                                      │        │         │
│      │    │ • Human authority zone                        │        │         │
│      │    │ • AI presents options                         │        │         │
│      │    │ • Human selects direction                     │        │         │
│      │    └────┬────┘                                      │        │         │
│      │         │                                           │        │         │
│      │         ▼                                           │        │         │
│      │    ┌─────────────────────────────────┐              │        │         │
│      │    │  CHECKPOINT: "Clear what to do?"│              │        │         │
│      │    │  Human confirms action          │              │        │         │
│      │    └────┬────────────────────────────┘              │        │         │
│      │         │                                           │        │         │
│      │         ▼                                           │        │         │
│      │    ┌─────────┐                                      │        │         │
│      │    │   DO    │                                      │        │         │
│      │    │         │                                      │        │         │
│      │    │ • Sub-agents execute                          │        │         │
│      │    │ • External tools called                       │        │         │
│      │    │ • Outputs generated                           │        │         │
│      │    └────┬────┘                                      │        │         │
│      │         │                                           │        │         │
│      │         ▼                                           │        │         │
│      │    ┌─────────────────────────────────┐              │        │         │
│      │    │  LOGGER records action          │──────────────┘        │         │
│      │    │  • What was done                │                       │         │
│      │    │  • By whom/what                 │                       │         │
│      │    │  • Result/output                │                       │         │
│      │    │  • Next steps                   │                       │         │
│      │    └─────────────────────────────────┘                       │         │
│      │                                                              │         │
│      │                        (cycle continues)                     │         │
│      └──────────────────────────────────────────────────────────────┘         │
│                                                                                │
└────────────────────────────────────────────────────────────────────────────────┘
```

---

## Skill Bundle Specification

### Overview

| Skill Name | Purpose | Inputs | Outputs | Integrations |
|------------|---------|--------|---------|--------------|
| `secretary_orchestrator` | Route requests, manage flow | User requests | Routed actions | All sub-skills |
| `project_brief` | Create project definitions | Ideas, brain dumps | Structured brief | Logger |
| `task_engine` | Manage tasks and subtasks | Brief, raw tasks | Organized todo | Taskmaster MCP |
| `delegation_advisor` | Recommend task assignments | Tasks from todo | Assignment + prompt | External agents |
| `project_logger` | Record all actions | Actions, outputs | Timestamped log | All skills |

---

### Skill 1: secretary_orchestrator

```yaml
SKILL: secretary_orchestrator
VERSION: 1.0

PURPOSE: |
  Central coordinator for the Secretary System. Recognizes intent,
  routes to appropriate sub-agent, manages ADD cycle, and aggregates
  feedback from all sub-agents.

TRIGGERS:
  explicit:
    - "מזכירה"
    - "secretary"
    - "PA"
  implicit:
    - "new project"
    - "I have an idea for"
    - "what's the status"
    - "what should I work on"
    - "delegate this"

WORKFLOW:
  1_recognize:
    action: "Parse user input to identify intent"
    intents:
      - PROJECT_INIT: "new project, idea, venture"
      - TASK_MANAGE: "task, todo, organize, prioritize"
      - STATUS_CHECK: "status, where are we, what's done"
      - DELEGATION: "delegate, assign, who should"
      - BRAIN_DUMP: "dump, capture, I'm thinking"
    output: "Identified intent + relevant project"

  2_context:
    action: "Load context from living documents"
    reads:
      - "{project}_brief.md"
      - "{project}_tasks.md"
      - "{project}_log.md"
    output: "Current project state"

  3_route:
    action: "Route to appropriate sub-agent"
    routing:
      PROJECT_INIT: "project_brief"
      TASK_MANAGE: "task_engine"
      STATUS_CHECK: "project_logger (read mode)"
      DELEGATION: "delegation_advisor"
      BRAIN_DUMP: "task_engine (capture mode)"

  4_checkpoint:
    action: "Present results and await human decision"
    pattern: "ADD Decide phase"

  5_aggregate:
    action: "Collect feedback from sub-agent"
    stores: "Learnings for future routing"

FEEDBACK_LOOP:
  description: |
    After each sub-agent completes, it returns feedback to the orchestrator:
    - What worked well
    - What was unclear
    - Suggested improvements
  storage: "Accumulated in orchestrator context for session"
```

---

### Skill 2: project_brief

```yaml
SKILL: project_brief
VERSION: 1.0

PURPOSE: |
  Transform project ideas and brain dumps into structured project
  definitions with clear scope, goals, and success criteria.

TRIGGERS:
  - "create brief for"
  - "new project:"
  - "define project"
  - Routed from orchestrator with PROJECT_INIT intent

INPUTS:
  required:
    - project_idea: "Raw description or brain dump"
  optional:
    - constraints: "Time, budget, resources"
    - stakeholders: "Who's involved"
    - existing_docs: "Related materials"

OUTPUT_TEMPLATE: |
  # Project Brief: {project_name}
  
  ## Overview
  **Created:** {date}
  **Status:** Active
  **Owner:** Omer
  
  ## Problem Statement
  {what_problem_does_this_solve}
  
  ## Goals
  1. {primary_goal}
  2. {secondary_goals}
  
  ## Scope
  ### In Scope
  - {included_items}
  
  ### Out of Scope
  - {excluded_items}
  
  ## Milestones
  | Milestone | Target Date | Status |
  |-----------|-------------|--------|
  | {milestone_1} | {date} | Not Started |
  
  ## Deliverables
  - {deliverable_1}
  - {deliverable_2}
  
  ## Success Criteria
  - {criterion_1}
  - {criterion_2}
  
  ## Constraints & Risks
  - {constraint_or_risk}
  
  ## Notes
  {additional_context}

WORKFLOW:
  1_extract:
    action: "Parse brain dump for key elements"
    extracts:
      - Core idea
      - Implicit goals
      - Mentioned constraints
      - Success indicators

  2_structure:
    action: "Organize into brief template"
    checkpoint: "Present draft, ask for additions"

  3_refine:
    action: "Incorporate human feedback"
    iterations: "Until human approves"

  4_save:
    action: "Save as {project}_brief.md"
    notify: "Logger to record creation"

FEEDBACK_TO_ORCHESTRATOR:
  - "Brief created for: {project_name}"
  - "Key goals identified: {count}"
  - "Suggested next step: Create task breakdown"
```

---

### Skill 3: task_engine

```yaml
SKILL: task_engine
VERSION: 1.0

PURPOSE: |
  Transform project briefs and brain dumps into organized, prioritized
  task lists with dependencies. Syncs bidirectionally with Taskmaster MCP.

TRIGGERS:
  - "create tasks for"
  - "organize these tasks"
  - "what needs to be done"
  - "brain dump:"
  - Routed from orchestrator with TASK_MANAGE or BRAIN_DUMP intent

INTEGRATIONS:
  taskmaster_mcp:
    read: "get_tasks, get_task, next_task"
    write: "parse_prd, expand_task, set_task_status, update_subtask"
    sync: "Bidirectional - local md ↔ Taskmaster"

INPUTS:
  required:
    - source: "Brief, brain dump, or raw task list"
  optional:
    - project_context: "From {project}_brief.md"
    - existing_tasks: "From {project}_tasks.md or Taskmaster"

OUTPUT_TEMPLATE: |
  # Tasks: {project_name}
  
  **Last Updated:** {timestamp}
  **Total Tasks:** {count} | **Completed:** {done_count}
  
  ## High Priority / Urgent
  
  ### [ ] {task_name}
  - **ID:** {task_id}
  - **Status:** {status}
  - **Assignee:** {human|ai_agent|pending}
  - **Dependencies:** {deps}
  - **Due:** {date}
  - **Subtasks:**
    - [ ] {subtask_1}
    - [ ] {subtask_2}
  
  ## Medium Priority
  ...
  
  ## Low Priority / Backlog
  ...
  
  ## Completed
  - [x] {completed_task} (Done: {date})

WORKFLOW:
  1_capture:
    action: "Collect all task items from input"
    modes:
      from_brief: "Extract implied tasks from milestones/deliverables"
      from_dump: "Parse brain dump for action items"
      from_list: "Clean up existing task list"

  2_structure:
    action: "Break into tasks and subtasks"
    rules:
      - "One clear outcome per task"
      - "Subtasks if >2 hours of work"
      - "Max 3 levels of nesting"

  3_analyze:
    action: "Identify dependencies and priority"
    priority_matrix:
      urgent_important: "High Priority"
      not_urgent_important: "Medium Priority"
      urgent_not_important: "Delegate"
      neither: "Low Priority / Backlog"

  4_sync:
    action: "Sync with Taskmaster MCP"
    operations:
      - "Create new tasks via parse_prd or direct creation"
      - "Update status via set_task_status"
      - "Expand complex tasks via expand_task"

  5_checkpoint:
    action: "Present organized list"
    question: "Does this capture everything? Priority correct?"

  6_save:
    action: "Save as {project}_tasks.md"
    notify: "Logger to record task creation/update"

FEEDBACK_TO_ORCHESTRATOR:
  - "Tasks organized: {count} total, {high_priority} high priority"
  - "Dependencies identified: {dep_count}"
  - "Suggested next: Run delegation_advisor on high priority tasks"
```

---

### Skill 4: delegation_advisor

```yaml
SKILL: delegation_advisor
VERSION: 1.0

PURPOSE: |
  Analyze tasks and recommend optimal delegation - whether to human,
  AI agent, or external tool. Generate appropriate prompts for AI delegation.

TRIGGERS:
  - "who should do this"
  - "delegate"
  - "assign tasks"
  - "what can AI do"
  - Routed from orchestrator with DELEGATION intent

AGENT_PROFILES:
  claude_code:
    strengths:
      - "Building software/tools"
      - "Coding and debugging"
      - "System architecture"
      - "Deployment and DevOps"
    best_for: "Implementation tasks, technical building"
    prompt_style: "Detailed specs, clear acceptance criteria"

  gemini:
    strengths:
      - "Deep research"
      - "Multi-document analysis"
      - "Long context processing"
      - "Comparative analysis"
    best_for: "Research tasks, document analysis"
    prompt_style: "Clear research questions, source guidance"

  chatgpt:
    strengths:
      - "Creative writing"
      - "Drafting and editing"
      - "Brainstorming"
      - "Conversational interfaces"
    best_for: "Content creation, ideation"
    prompt_style: "Creative briefs, tone guidance"

  human_omer:
    strengths:
      - "Strategic decisions"
      - "Stakeholder communication"
      - "Quality judgment"
      - "Domain expertise application"
    best_for: "Decisions, reviews, external communication"
    requires: "Clear options and recommendations"

  taskmaster:
    strengths:
      - "Task tracking"
      - "Dependency management"
      - "Progress monitoring"
    best_for: "Task CRUD operations, status updates"
    integration: "MCP direct calls"

  other_mcps:
    monday: "Project boards, team collaboration"
    google_drive: "Document storage, sharing"
    gmail: "Email communication"

WORKFLOW:
  1_analyze:
    action: "Assess task requirements"
    factors:
      - "Technical complexity"
      - "Creative requirements"
      - "Decision authority needed"
      - "External communication"
      - "Time sensitivity"

  2_match:
    action: "Match to best agent"
    output:
      assignee: "{agent_name}"
      confidence: "{high|medium|low}"
      reasoning: "{why_this_agent}"

  3_generate_prompt:
    action: "Create handoff prompt for AI agents"
    template: |
      ## Task Handoff: {task_name}
      
      ### Context
      {project_context_summary}
      
      ### Objective
      {clear_task_description}
      
      ### Requirements
      - {requirement_1}
      - {requirement_2}
      
      ### Acceptance Criteria
      - {criterion_1}
      - {criterion_2}
      
      ### Constraints
      - {constraint_1}
      
      ### Output Format
      {expected_deliverable_format}

  4_checkpoint:
    action: "Present recommendation to human"
    question: "Approve this delegation? Modify assignee?"

  5_update:
    action: "Update task with assignee"
    writes_to: "{project}_tasks.md"
    notify: "Logger to record delegation"

FEEDBACK_TO_ORCHESTRATOR:
  - "Delegation recommended: {task} → {agent}"
  - "Prompt generated: {word_count} words"
  - "Confidence: {level}"
```

---

### Skill 5: project_logger

```yaml
SKILL: project_logger
VERSION: 1.0

PURPOSE: |
  Maintain a running log of all project activities, decisions, and outputs.
  Serves as the project's institutional memory and feeds context back
  to the orchestrator.

TRIGGERS:
  - "log this"
  - "record that"
  - "what happened with"
  - "project history"
  - Auto-triggered by other skills after actions

MODES:
  write:
    trigger: "After any action by any skill"
    auto: true

  read:
    trigger: "Status check, context loading"
    returns: "Relevant log entries"

  summarize:
    trigger: "Project overview request"
    returns: "Condensed project history"

OUTPUT_TEMPLATE: |
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
  
  ## {previous_date}
  ...

LOG_ENTRY_TYPES:
  - PROJECT_CREATED: "New project brief created"
  - TASKS_DEFINED: "Task list created or updated"
  - TASK_COMPLETED: "Specific task marked done"
  - TASK_DELEGATED: "Task assigned to agent"
  - DECISION_MADE: "Human made a decision"
  - BLOCKER_IDENTIFIED: "Issue blocking progress"
  - BLOCKER_RESOLVED: "Issue resolved"
  - MILESTONE_REACHED: "Major milestone completed"
  - OUTPUT_DELIVERED: "Deliverable produced"

WORKFLOW:
  write_mode:
    1_receive:
      action: "Receive action notification from skill"
      data:
        - skill_name
        - action_type
        - action_details
        - outputs
        - next_steps

    2_format:
      action: "Structure into log entry"
      adds:
        - timestamp
        - formatted_entry

    3_append:
      action: "Append to {project}_log.md"
      position: "Top (newest first)"

  read_mode:
    1_query:
      action: "Parse what information is needed"
      types:
        - recent: "Last N entries"
        - by_type: "All entries of type X"
        - by_date: "Entries from date range"
        - search: "Entries containing keyword"

    2_retrieve:
      action: "Extract relevant entries"
      returns: "Formatted log excerpt"

  summarize_mode:
    1_analyze:
      action: "Process full log"
      extracts:
        - "Key milestones"
        - "Major decisions"
        - "Current blockers"
        - "Recent activity"

    2_condense:
      action: "Generate summary"
      format: |
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

FEEDBACK_TO_ORCHESTRATOR:
  - "Log entry created: {entry_type}"
  - "Project health: {status}"
  - "Days since last activity: {count}"
```

---

## Implementation Guide

### Phase 1: Build Core Secretary (Once)

1. **Create skill bundle in Claude Code**
   ```
   /mnt/skills/user/secretary/
   ├── SKILL.md                      # Main orchestrator
   ├── sub-skills/
   │   ├── project_brief/SKILL.md
   │   ├── task_engine/SKILL.md
   │   ├── delegation_advisor/SKILL.md
   │   └── project_logger/SKILL.md
   └── templates/
       ├── brief_template.md
       ├── tasks_template.md
       └── log_template.md
   ```

2. **Configure Taskmaster MCP**
   - Ensure Taskmaster MCP is connected
   - Verify it works in both Claude AI and Claude Code

3. **Test Core functionality**
   - Verify skills load in Claude AI
   - Verify agent runs in Claude Code

### Phase 2: Instantiate Per Project

For each new project (e.g., "AI Strategy - Law Faculty"):

**Option A: Claude AI Project**
1. Create new Claude AI Project
2. Add Project Instructions:
   ```
   This project uses the Secretary System skill bundle.
   
   Invoke with: "מזכירה" or "secretary"
   
   Living Documents (attached as Knowledge):
   - brief.md - Project definition
   - tasks.md - Task list
   - log.md - Activity log
   ```
3. Secretary will create living documents on first use

**Option B: Claude Code Workspace**
1. Create project directory
2. Initialize with Taskmaster: `taskmaster init`
3. Secretary agent will create living documents on first use

### Phase 3: Pilot Project

**Recommended Pilot:** AI Strategy for Law Faculty

1. Create initial brief from existing materials
2. Generate task breakdown
3. Run delegation advisor on first batch
4. Test logging through one complete ADD cycle
5. Test in **both** Claude AI and Claude Code to verify cross-interface compatibility

---

## Usage Examples

### Example 1: Starting a New Project

```
User: "מזכירה, יש לי רעיון לפרויקט חדש - לבנות framework לאסטרטגיית AI 
       לפקולטה למשפטים. רוצה להציג את זה לדיקן."

Orchestrator: 
  → Recognizes: PROJECT_INIT intent
  → Routes to: project_brief skill
  → Checkpoint: "Let me create a brief. What's the timeline and who 
                 are the stakeholders?"
  
[After brief creation]

Orchestrator:
  → Suggests: "Brief created. Should I break this into tasks?"
  
[User approves]

Orchestrator:
  → Routes to: task_engine
  → Creates task list with Taskmaster sync
  → Checkpoint: "Here are 12 tasks. Review priority?"
```

### Example 2: Status Check

```
User: "מזכירה, מה הסטטוס של פרויקט האסטרטגיה?"

Orchestrator:
  → Recognizes: STATUS_CHECK intent
  → Routes to: project_logger (read mode)
  → Returns: Summary of recent activity, blockers, next steps
```

### Example 3: Delegation Request

```
User: "מזכירה, מי יכול לעשות את המחקר על frameworks קיימים?"

Orchestrator:
  → Recognizes: DELEGATION intent
  → Routes to: delegation_advisor
  → Analysis: "Research task, multi-document, comparative"
  → Recommendation: "Gemini - best for deep research"
  → Generated prompt for Gemini handoff
  → Checkpoint: "Approve delegation to Gemini with this prompt?"
```

---

## Document Metadata

```yaml
document:
  title: "Secretary System Architecture"
  version: "1.1"
  date: "December 2025"
  author: "Omer + Claude (Collaborative Design)"
  
  purpose:
    - "Define personal assistant skill bundle"
    - "Integrate with existing Context Architecture v4"
    - "Enable project management outside academic scope"
    - "Provide cross-interface compatibility (Claude AI + Claude Code)"
  
  architecture:
    model: "Build Once, Instantiate Per Project"
    core_location: "/mnt/skills/user/secretary/"
    instance_location: "Claude AI Project Knowledge OR filesystem"
    interfaces: ["Claude AI (Skill)", "Claude Code (Agent)"]
  
  dependencies:
    - "Context Architecture Blueprint v4"
    - "Taskmaster MCP"
    - "Claude AI Projects"
    - "Claude Code"
  
  changelog:
    v1.1:
      - "Added Core/Instance architecture separation"
      - "Added cross-interface compatibility table"
      - "Clarified skill vs agent behavior per interface"
      - "Updated implementation guide for Build Once model"
    v1.0:
      - "Initial architecture design"
  
  status: "Ready for Implementation"
```

---

*End of Secretary System Architecture v1.1*
