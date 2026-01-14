---
tags: [dashboard]
---

# Tech Leadership Dashboard

*Last updated: `= date(now)`*

## 🎯 My Kanban Board

### 🏗️ DOING (Focus Now)
```dataview
TABLE priority as "Pri", project as "Theme"
FROM "01-Active"
WHERE status = "doing"
SORT priority DESC, file.mtime DESC
```

### 📋 TO DO (Up Next)
```dataview
TABLE priority as "Pri", project as "Theme"
FROM "01-Active"
WHERE status = "todo"
SORT priority DESC
LIMIT 10
```

### 👀 IN REVIEW
```dataview
TABLE priority as "Pri", project as "Theme"
FROM "01-Active"
WHERE status = "review"
SORT file.mtime DESC
```

### ✅ DONE THIS WEEK
```dataview
TABLE project as "Theme", file.mtime as "Completed"
FROM "01-Active"
WHERE status = "done"
AND file.mtime >= date(today) - dur(7 days)
SORT file.mtime DESC
LIMIT 15
```

---

## 📊 Active Themes/Projects

```dataview
TABLE 
  status,
  length(file.outlinks) as "Links"
FROM "02-Projects"
WHERE status = "active"
SORT file.mtime DESC
```

### Quick Theme Links
- [[02-Projects/Fraud-Detection/Overview]]
- [[02-Projects/Platform-Evaluation/Overview]]
- [[02-Projects/Camunda-Integration/Overview]]

---

## 🧠 Recent Learning (Last 14 Days)
```dataview
TABLE topic, source
FROM "03-Learning"
WHERE file.mtime >= date(today) - dur(14 days)
SORT file.mtime DESC
LIMIT 10
```

---

## ✍️ Content Pipeline

### LinkedIn - Drafts in Progress
```dataview
TABLE status, theme, target-audience as "Audience"
FROM "04-Content/LinkedIn"
WHERE status = "draft"
SORT date DESC
```

### LinkedIn - Ready to Publish
```dataview
TABLE date as "Target Date", theme
FROM "04-Content/LinkedIn"
WHERE status = "ready"
SORT date ASC
```

### Internal Content - In Progress
```dataview
TABLE type, audience, status
FROM "04-Content/Internal"
WHERE status != "published"
SORT file.mtime DESC
```

---

## 🔧 Pattern Library
```dataview
TABLE domain, stack, file.mtime as "Updated"
FROM "05-Patterns"
SORT file.mtime DESC
LIMIT 10
```

---

## 📥 Inbox to Process
```dataview
LIST
FROM "00-Inbox"
WHERE file.mtime >= date(today) - dur(3 days)
SORT file.mtime DESC
```

---

## 📈 Weekly Stats

### Work Distribution
```dataview
TABLE 
  length(rows) as "Count",
  choice(status = "done", "✅", choice(status = "doing", "🏗️", choice(status = "todo", "📋", "📥"))) as "Status"
FROM "01-Active"
GROUP BY status
```

### Content Production (Last 30 Days)
- LinkedIn Posts: `= length(filter(file.lists.file, (f) => contains(f.path, "LinkedIn") AND f.mtime >= date(today) - dur(30 days)))`
- Internal Docs: `= length(filter(file.lists.file, (f) => contains(f.path, "Internal") AND f.mtime >= date(today) - dur(30 days)))`
- Patterns: `= length(filter(file.lists.file, (f) => contains(f.path, "Patterns") AND f.mtime >= date(today) - dur(30 days)))`

---

## 🚀 Quick Actions

- [[00-Inbox/Quick Capture]] - Brain dump here
- Create new task: Use template `[[06-Templates/Kanban-Task]]`
- Create theme: Use template `[[06-Templates/Theme-Area]]`
- Start learning note: Use template `[[06-Templates/Learning-Note]]`

---

## 🎯 This Week's Focus
<!-- Manually update this each week -->

**Theme:** 

**Key Task:** 

**Content Goal:**

---

**Keyboard Shortcuts:**
- `Cmd/Ctrl + O` - Quick switcher
- `Cmd/Ctrl + P` - Command palette  
- `Cmd/Ctrl + N` - New note
- `Cmd/Ctrl + T` - New task (with Tasks plugin)
