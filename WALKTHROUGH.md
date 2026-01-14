# Obsidian Vault Walkthrough

Step-by-step visual guide to using your Tech Leadership vault with kanban workflow.

## Initial Setup (5 minutes)

### Step 1: Install Obsidian
1. Download from https://obsidian.md
2. Install and open Obsidian
3. Click "Open folder as vault"
4. Navigate to extracted `tech-leadership-vault` folder
5. Click "Open"

**First view**: You'll see the folder structure in the left sidebar.

### Step 2: Install Required Plugins
1. Click Settings icon (⚙️) in bottom-left
2. Go to "Community plugins" → "Turn on community plugins"
3. Click "Browse" and install these plugins:
   - **Dataview** (for dynamic queries)
   - **Tasks** (for task management)
   - **Templater** (for templates)
   - **Calendar** (for date navigation)
   - **Obsidian Git** (for GitHub sync)
4. Enable each plugin after installation

**This enables**: Dynamic dashboards, kanban boards, and GitHub sync.

### Step 3: Open Dashboard
1. Press `Cmd/Ctrl + O` (Quick Switcher)
2. Type "Dashboard"
3. Press Enter

**You'll see**: Your main dashboard with empty kanban boards and content pipeline.

---

## Daily Workflow (10-15 minutes/day)

### Morning: Check Your Dashboard

#### What You See
```
🎯 My Kanban Board
├── 🏗️ DOING (Focus Now)        ← Your active work
├── 📋 TO DO (Up Next)            ← Queued tasks
├── 👀 IN REVIEW                  ← Awaiting feedback
└── ✅ DONE THIS WEEK             ← Recent completions

📊 Active Themes/Projects         ← Your major work areas

✍️ Content Pipeline
├── LinkedIn - Drafts             ← Posts in progress
└── Internal Content              ← Capgemini docs
```

#### Action: Review "DOING" Tasks
- See what you're actively working on
- Click any task to open full details
- Update progress in the task note

---

### Creating a New Task

#### Step 1: Open Template
1. Press `Cmd/Ctrl + P` (Command Palette)
2. Type "Templater: Create new note from template"
3. Select "Kanban-Task"

#### Step 2: Fill in Task Details
```markdown
---
status: todo          ← Set status: backlog/todo/doing/review/done
priority: high        ← Set priority: high/medium/low
project: Fraud-Detection  ← Link to your project/theme
---

# Research Camunda Retry Patterns

## Description
Research retry configuration for AI service timeouts

## Acceptance Criteria
- [ ] Document retry options
- [ ] Test with Bedrock
- [ ] Create code example
```

#### Step 3: Save and Close
- Press `Cmd/Ctrl + S` to save
- Task automatically appears in Dashboard under "TO DO"

**Visual Flow**:
```
Create from Template → Fill Details → Save → Appears in Dashboard
```

---

### Working on a Task

#### Moving Through Kanban States

**To start working on a task:**
1. Open the task note
2. Change `status: todo` to `status: doing`
3. Save

**The task automatically moves** from "TO DO" section to "DOING" section in Dashboard.

**Status flow:**
```
📥 backlog → 📋 todo → 🏗️ doing → 👀 review → ✅ done
```

#### Adding Notes While Working
```markdown
## Notes

### Key Findings (2026-01-13)
- Long polling works better than HTTP for AI latency
- Built-in retry with exponential backoff
- Each fraud model can be separate task topic

### Architecture Insight
[Draw or paste diagrams here]
```

**Links to capture:**
```markdown
## Resources
- [[03-Learning/Workflow-Patterns]] ← Link to learning note
- [[05-Patterns/Camunda-AI-Pattern]] ← Link to pattern
- https://docs.camunda.io/... ← External references
```

---

### Capturing Learning

#### While Working, Create Learning Notes

**Quick capture** (for later processing):
1. Press `Cmd/Ctrl + O`
2. Open "00-Inbox/Quick Capture"
3. Add bullet points:
```markdown
## 2026-01-13

- Key insight about Camunda retry patterns #learning #camunda
  - Long polling is better than HTTP
  - This solves our timeout issue!
  
- Interesting approach to multi-model routing #learning #fraud-detection
  - Each model gets own task topic
  - Camunda handles the routing
```

**Formal learning note** (from template):
1. Use "Learning-Note" template
2. Document detailed findings
3. Link from task: `[[03-Learning/Camunda-Retry-Patterns]]`

**Result**: Knowledge is captured and linked to your work context.

---

### Creating Content

#### Path: Learning → Internal Doc → Public Post

**Example scenario**: You've learned about Camunda + AI integration while building fraud detection.

#### Step 1: Internal Solution Paper
1. Use "Internal-Solution-Paper" template
2. Create: `04-Content/Internal/Fleet-Fraud-Detection-Solution.md`
3. Document:
   - Business context
   - Technical approach  
   - Client success story (sanitized)
   - Reusability framework

#### Step 2: LinkedIn Post
1. Use "LinkedIn-Post" template
2. Create: `04-Content/LinkedIn/When-Workflows-Meet-Agents.md`
3. Extract public-facing insights:
   - Hook: "Most AI agents lack discipline"
   - Your experience with Camunda + AI
   - Key pattern or insight
   - Question for engagement

#### Visual in Dashboard
```
✍️ Content Pipeline

LinkedIn - Drafts in Progress
├── When-Workflows-Meet-Agents  [status: draft]
└── Fleet-Intelligence-Patterns [status: draft]

LinkedIn - Ready to Publish
└── Platform-Decision-Framework [status: ready]

Internal Content - In Progress  
└── Fleet-Fraud-Detection-Solution [status: draft]
```

---

### Using Project/Theme Boards

#### View a Project's Kanban

**Example: Open Fraud Detection project**
1. Press `Cmd/Ctrl + O`
2. Type "Fraud-Detection Overview"
3. Open the note

**You see embedded kanban board** showing only tasks for this project:
```
📥 Backlog
├── Task A
└── Task B

📋 To Do
└── Task C

🏗️ Doing
└── Task D ← Currently working

✅ Done (Last 30 days)
├── Completed task 1
└── Completed task 2
```

**Plus project context:**
- Goals
- Current focus
- Patterns developed
- Content generated
- Learning captured

**This view**: Shows all work related to one theme/project.

---

## Weekly Review Workflow

### End of Week: Review & Plan

#### Step 1: Check Dashboard "DONE THIS WEEK"
See everything completed - celebrate wins!

#### Step 2: Move Unfinished Work
- Tasks in "DOING" → decide: continue or back to "TODO"
- Tasks in "REVIEW" → finalize or iterate

#### Step 3: Plan Next Week
Open "00-Inbox/Quick Capture" and brainstorm:
```markdown
## Next Week Planning

Priority tasks:
- [ ] Complete Camunda pattern doc
- [ ] Draft LinkedIn post
- [ ] Start platform evaluation research

New ideas:
- Research Bedrock vs Azure comparison
- Connect with Camunda team on agent patterns
```

#### Step 4: Create Tasks from Planning
- Use Kanban-Task template for each priority
- Set status: `backlog` or `todo`
- Assign to projects/themes

---

## Content Creation Workflow

### Full Cycle: Project → Learning → Content

#### Week 1: Active Work
```
Working on: Fraud Detection + Camunda Integration
       ↓
Capture learning as you go
       ↓
Link tasks to learning notes
```

#### Week 2: Synthesis
```
Review project work
       ↓
Extract patterns → 05-Patterns/
       ↓
Create internal doc → 04-Content/Internal/
       ↓
Draft LinkedIn post → 04-Content/LinkedIn/
```

#### Visual Example

**From task note:**
```markdown
# Camunda External Task Research

## Notes
- Discovered: Long polling pattern for AI services
- This solves latency issues
- Created: [[05-Patterns/Camunda-AI-External-Task]]
```

**Pattern doc created:**
```markdown
# Pattern: Camunda AI External Task

[Detailed technical pattern]

Applied in: [[02-Projects/Fraud-Detection/Overview]]
```

**Internal doc:**
```markdown
# Fleet Fraud Detection Solution

Uses pattern: [[05-Patterns/Camunda-AI-External-Task]]
```

**LinkedIn post:**
```markdown
# When Workflows Meet Agents

Based on: [[05-Patterns/Camunda-AI-External-Task]]
For: Technical architects facing agent chaos
```

**Result**: One learning → Multiple valuable outputs

---

## Advanced: Using MCP with Claude

### What MCP Enables

**Without MCP:**
1. Work in Obsidian
2. Copy content
3. Paste to Claude
4. Copy response
5. Paste back to Obsidian

**With MCP:**
1. Tell Claude what you need
2. Claude reads your vault for context
3. Claude creates/updates notes directly
4. Everything stays in sync

### Example Interaction

**You (in Claude):** "Create a LinkedIn post about the Camunda + AI work"

**Claude (using MCP):**
1. Reads `02-Projects/Fraud-Detection/Overview.md`
2. Reads `03-Learning/Camunda-Retry-Patterns.md`
3. Reads past LinkedIn posts for your style
4. Generates post following your template
5. Saves to `04-Content/LinkedIn/` with proper metadata

**You see in Obsidian:** New post appears in your content pipeline, ready for review.

---

## Keyboard Shortcuts Reference

### Essential Shortcuts
- `Cmd/Ctrl + O` - Quick Switcher (open any note)
- `Cmd/Ctrl + P` - Command Palette (all commands)
- `Cmd/Ctrl + N` - New note
- `Cmd/Ctrl + F` - Search current note
- `Cmd/Ctrl + Shift + F` - Search all notes
- `Cmd/Ctrl + Click` - Open link in new pane
- `Cmd/Ctrl + E` - Toggle edit/preview mode

### Navigation
- `Cmd/Ctrl + B` - Toggle sidebar
- `Cmd/Ctrl + \` - Toggle right sidebar
- `Cmd/Ctrl + Tab` - Next note
- `Cmd/Ctrl + Shift + Tab` - Previous note

### Editing
- `Cmd/Ctrl + [` - Decrease indent
- `Cmd/Ctrl + ]` - Increase indent
- `Cmd/Ctrl + /` - Toggle comment
- `Cmd/Ctrl + D` - Delete current line

---

## Tips & Tricks

### Tip 1: Quick Task Creation
Create a hotkey for "Templater: Create new note from template" and select Kanban-Task. One keystroke → new task.

### Tip 2: Daily Dashboard Check
Bookmark `Dashboard.md` and make it your homepage:
- Settings → Core plugins → Daily notes → Open on startup: Dashboard

### Tip 3: Tag Strategy
Use hierarchical tags for better organization:
```markdown
#content/linkedin
#content/internal
#learning/agents
#learning/workflows
#project/fraud-detection
```

Dataview can then query: `WHERE contains(file.tags, "content")`

### Tip 4: Link Everything
The more you link, the more valuable your vault becomes:
- Link tasks to projects
- Link learning to tasks
- Link content to patterns
- Link patterns to learning

Creates a knowledge graph you can traverse.

### Tip 5: Archive Regularly
Move completed projects to `99-Archive/` to keep active workspace clean.

### Tip 6: Template Variables
Templater supports variables like:
- `{{date}}` - Current date
- `{{time}}` - Current time  
- `{{title}}` - Note title
- Custom JavaScript for dynamic content

### Tip 7: Mobile Sync
Obsidian has mobile apps. Use Obsidian Sync (paid) or GitHub + Working Copy (iOS) for mobile access.

---

## Common Workflows

### Workflow: Daily Standup Prep
1. Open Dashboard
2. Check "DOING" section
3. Review "DONE THIS WEEK"
4. Update task notes with progress

**Time**: 5 minutes

### Workflow: Weekly Planning
1. Review Dashboard completions
2. Brainstorm in Quick Capture
3. Create tasks for next week
4. Prioritize: Set high/medium/low
5. Review content pipeline

**Time**: 30 minutes

### Workflow: Monthly Review
1. Check all project boards
2. Archive completed projects
3. Review learning captured
4. Plan content themes for next month
5. Update project goals

**Time**: 1 hour

### Workflow: Content Creation Sprint
1. Pick theme from projects
2. Review all related notes (tasks, learning, patterns)
3. Create internal doc using template
4. Extract LinkedIn post using template
5. Set status: draft → ready → published

**Time**: 2-4 hours

---

## Troubleshooting

### Dataview Queries Not Working
**Symptom**: Dashboard shows empty or "No results"
**Fix**: 
1. Check Dataview plugin is enabled
2. Verify frontmatter format (YAML between `---`)
3. Check field names match queries exactly

### Tasks Not Appearing in Dashboard
**Symptom**: Created task but not showing
**Fix**:
1. Verify task is in `01-Active/` folder
2. Check frontmatter has `status: [value]`
3. Refresh dashboard (close and reopen)

### Git Sync Issues
**Symptom**: Obsidian Git plugin errors
**Fix**:
1. Check git is initialized: `git status` in vault folder
2. Verify remote is set: `git remote -v`
3. Check for merge conflicts

### Templates Not Available
**Symptom**: Can't find templates in command palette
**Fix**:
1. Settings → Core plugins → Templates → Enable
2. Set template folder: `06-Templates`
3. Reload Obsidian

---

## Next Steps

### Week 1: Learn the System
- [ ] Set up vault with plugins
- [ ] Create 3 tasks using template
- [ ] Move tasks through kanban states
- [ ] Create 1 learning note

### Week 2: Build Content
- [ ] Complete 1 internal doc
- [ ] Draft 1 LinkedIn post
- [ ] Document 1 pattern
- [ ] Review weekly in Dashboard

### Week 3: Optimize
- [ ] Add custom keyboard shortcuts
- [ ] Customize Dashboard queries
- [ ] Set up Git auto-sync
- [ ] Try MCP integration (optional)

### Month 2: Establish Rhythm
- [ ] 2-week content cycle working
- [ ] Pattern library growing
- [ ] Learning captured regularly
- [ ] Public content publishing

---

## Resources

- **Obsidian Forum**: https://forum.obsidian.md/
- **Dataview Docs**: https://blacksmithgu.github.io/obsidian-dataview/
- **Tasks Plugin**: https://github.com/obsidian-tasks-group/obsidian-tasks
- **Templater**: https://silentvoid13.github.io/Templater/

---

**Questions?** Open `00-Inbox/Quick Capture` and jot them down. Process later or ask in Obsidian community.

**Ready to start?** Extract the vault ZIP, open in Obsidian, and begin with the Dashboard!
