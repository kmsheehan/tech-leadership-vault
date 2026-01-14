---
created: 2026-01-13
status: doing
priority: high
project: Fraud-Detection
tags: [task]
---

# Research Camunda External Task Patterns for AI Integration

## Description
Research and document best practices for using Camunda external task pattern with long-running AI service calls (AWS Bedrock). Focus on timeout handling, retry logic, and state management.

## Context
The fraud detection workflow needs to call AI services that may take several seconds to respond. Need to understand how to properly handle this in Camunda without blocking the workflow engine.

Related to: [[02-Projects/Fraud-Detection/Overview]]

## Acceptance Criteria
- [x] Review Camunda docs on external task patterns
- [x] Identify best practices for async AI calls
- [ ] Document retry configuration options
- [ ] Create architecture diagram
- [ ] Validate approach with Penske team

## Notes

### Key Findings (2026-01-13)
- **Long polling** is better than HTTP request/response for AI latency
- External task workers can be scaled independently
- Built-in retry with exponential backoff
- Each fraud model can be a separate task topic
- State is maintained in process variables

### Architecture Insight
```
Fleet Data → Camunda Process
              ↓
         External Task Created (topic: "fraud-analysis")
              ↓
         Task Worker polls Camunda
              ↓
         Worker calls Bedrock API
              ↓
         Worker updates task with result
              ↓
         Process continues with result
```

### Retry Configuration
```yaml
lockDuration: 60000  # 60 seconds for AI call
retries: 3
retryTimeout: 120000 # 2 minutes between retries
```

## Resources
- [Camunda External Tasks Docs](https://docs.camunda.io/docs/components/concepts/external-tasks/)
- [[03-Learning/Workflow-Orchestration-Patterns]] (to be created)

## Time Investment
- Estimated: 4 hours
- Actual: 3 hours so far

## Next Actions
- [ ] Create detailed architecture diagram
- [ ] Build proof-of-concept external task worker
- [ ] Document as pattern in [[05-Patterns/]]
- [ ] Share findings in team meeting

---
**Status Flow:** backlog → todo → doing → review → done
**Project:** [[02-Projects/Fraud-Detection/Overview]]
**Created:** 2026-01-13
