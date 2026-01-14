---
created: 2026-01-13
theme: fraud-detection
status: active
tags: [project, theme]
---

# Theme: Fraud Detection in Fleet Rental

## Overview
Building Gen AI-powered fraud detection system for fleet and consumer rental at Penske. Integrating with Camunda workflows to provide orchestrated, auditable fraud analysis with human-in-the-loop review.

## Goals
- Design agent framework for fraud pattern detection
- Integrate with Camunda workflow orchestration
- Create reusable patterns for automotive/fleet domain
- Generate thought leadership content on workflow + AI integration

## Current Focus
Designing external task pattern for Camunda + AWS Bedrock integration that handles AI service timeouts and provides structured retry logic.

## Kanban Board

### 📥 Backlog
```dataview
TABLE priority, project as "Related"
FROM "01-Active"
WHERE contains(project, "Fraud-Detection") AND status = "backlog"
SORT priority DESC
```

### 📋 To Do
```dataview
TABLE priority, project as "Related"
FROM "01-Active"
WHERE contains(project, "Fraud-Detection") AND status = "todo"
SORT priority DESC
```

### 🏗️ Doing
```dataview
TABLE priority, project as "Related"
FROM "01-Active"
WHERE contains(project, "Fraud-Detection") AND status = "doing"
SORT file.mtime DESC
```

### 👀 Review
```dataview
TABLE priority, project as "Related"
FROM "01-Active"
WHERE contains(project, "Fraud-Detection") AND status = "review"
```

### ✅ Done (Last 30 days)
```dataview
TABLE priority, file.mtime as "Completed"
FROM "01-Active"
WHERE contains(project, "Fraud-Detection") AND status = "done"
AND file.mtime >= date(today) - dur(30 days)
SORT file.mtime DESC
```

## Key Patterns Developed
- [[05-Patterns/Camunda-AI-External-Task-Pattern]] (planned)
- [[05-Patterns/Multi-Model-Fraud-Detection]] (planned)

## Learning Captured
- [[03-Learning/Agent-State-Management]] (planned)
- [[03-Learning/Workflow-Orchestration-Patterns]] (planned)

## Content Generated
- Internal: [[04-Content/Internal/Fleet-Fraud-Detection-Solution]] (planned)
- Public: [[04-Content/LinkedIn/When-Workflows-Meet-Agents]] (planned)

## Business Value
**Problem Solved:** Manual fraud detection is slow and inconsistent; pure AI agents lack auditability and orchestration.

**Solution:** Camunda provides workflow discipline + auditability; Gen AI provides intelligent pattern detection.

**Impact:** Faster detection, consistent process, clear audit trail, scalable to multiple fraud types.

## Technical Stack
- **Workflow Engine:** Camunda 8
- **AI Platform:** AWS Bedrock (Claude Sonnet)
- **Integration:** External Task Workers (Java/Node)
- **Data Sources:** Fleet rental transaction data

## Next Steps
1. Complete external task pattern architecture
2. Build reference implementation
3. Document as internal solution paper
4. Extract public thought leadership content

---
**Related Projects:** [[02-Projects/Camunda-Integration/Overview]]
**Dashboard:** [[Dashboard]]
