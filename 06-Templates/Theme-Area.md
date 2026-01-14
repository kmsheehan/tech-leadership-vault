---
created: {{date:YYYY-MM-DD}}
theme: 
status: active
tags: [project, theme]
---

# Theme: {{title}}

## Overview
<!-- What is this theme about? What business value does it deliver? -->

## Goals
- 
- 
- 

## Current Focus
<!-- What are you actively working on in this theme? -->

## Kanban Board

### 📥 Backlog
```dataview
TABLE priority, project as "Related"
FROM "01-Active"
WHERE contains(project, this.file.name) AND status = "backlog"
SORT priority DESC
```

### 📋 To Do
```dataview
TABLE priority, project as "Related"
FROM "01-Active"
WHERE contains(project, this.file.name) AND status = "todo"
SORT priority DESC
```

### 🏗️ Doing
```dataview
TABLE priority, project as "Related"
FROM "01-Active"
WHERE contains(project, this.file.name) AND status = "doing"
SORT file.mtime DESC
```

### 👀 Review
```dataview
TABLE priority, project as "Related"
FROM "01-Active"
WHERE contains(project, this.file.name) AND status = "review"
```

### ✅ Done (Last 30 days)
```dataview
TABLE priority, file.mtime as "Completed"
FROM "01-Active"
WHERE contains(project, this.file.name) AND status = "done"
AND file.mtime >= date(today) - dur(30 days)
SORT file.mtime DESC
```

## Key Patterns Developed
<!-- Link to patterns created from this work -->
- [[05-Patterns/]]

## Learning Captured
<!-- Link to learning notes -->
- [[03-Learning/]]

## Content Generated
<!-- Internal and external content -->
- Internal: [[04-Content/Internal/]]
- Public: [[04-Content/LinkedIn/]]

## Next Steps
<!-- What's coming next for this theme? -->

---
**Related Projects:** [[02-Projects/]]
**Dashboard:** [[Dashboard]]
