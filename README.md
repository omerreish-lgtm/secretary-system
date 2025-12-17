# Secretary System Project

מרכז כל הקבצים הקשורים לפרויקט Secretary System.

## מבנה התיקייה

```
secretary-system/
├── architecture/          # מסמך הארכיטקטורה המלא
│   └── secretary_system_architecture.md
├── plan/                  # תוכנית הביצוע
│   └── secretary_system_build_fb90150f.plan.md
├── agents/                # Cursor Agents לעבודה במקביל
│   ├── skill-orchestrator-builder.md
│   ├── references-builder.md
│   └── templates-scripts-builder.md
└── taskmaster/            # Taskmaster PRD ו-tasks
    ├── prd.txt
    └── tasks.json
```

## שימוש

ה-agents מוגדרים גם ב-`.claude/agents/` לשימוש ב-Cursor.

להפעלת ה-agents:
```
> Use skill-orchestrator-builder to create SKILL.md
> Use references-builder to create all 4 reference files
> Use templates-scripts-builder to create templates and scripts
```
