# Tech Leadership Vault

Obsidian vault for managing tech leadership work with a kanban-based workflow. Designed for capturing learning, building patterns, and creating thought leadership content.

## Structure

```
tech-leadership-vault/
├── 00-Inbox/              # Quick captures, process later
├── 01-Active/             # Active kanban tasks (backlog→todo→doing→review→done)
├── 02-Projects/           # Project/theme areas with embedded kanban boards
├── 03-Learning/           # Knowledge capture and learning notes
├── 04-Content/            # Content pipeline
│   ├── Internal/          # Capgemini solution papers, newsletter contributions
│   ├── LinkedIn/          # Public thought leadership posts
│   └── Newsletter/        # Architecture newsletter drafts
├── 05-Patterns/           # Reusable architecture patterns
├── 06-Templates/          # Note templates
└── 99-Archive/            # Completed work
```

## Quick Start

1. **Open Dashboard** - Your main view with kanban boards and content pipeline
2. **Use Quick Capture** - Brain dump in `00-Inbox/Quick Capture.md`
3. **Create Tasks** - Use `06-Templates/Kanban-Task.md` template
4. **Organize by Theme** - Use `06-Templates/Theme-Area.md` for projects

## Workflows

### Kanban Task Flow
1. Create task from template
2. Set status: `backlog` | `todo` | `doing` | `review` | `done`
3. Link to project/theme
4. Dashboard automatically organizes by status

### Content Pipeline
Learning → Internal Doc → Public Post
- Capture learning in `03-Learning/`
- Create internal solution paper in `04-Content/Internal/`
- Extract public post for `04-Content/LinkedIn/`
- Extract reusable pattern for `05-Patterns/`

## Required Plugins

Install these from Community Plugins:
1. **Dataview** - Dynamic content queries for dashboards
2. **Tasks** - Enhanced task management
3. **Templater** - Advanced templates
4. **Calendar** - Date-based navigation
5. **Obsidian Git** - Auto-sync to GitHub

## Templates Available

- **Kanban-Task.md** - Individual work items
- **Theme-Area.md** - Project/theme with embedded kanban board
- **Learning-Note.md** - Capture technical learning
- **LinkedIn-Post.md** - Public content creation
- **Architecture-Pattern.md** - Reusable technical patterns
- **Internal-Solution-Paper.md** - Capgemini knowledge sharing

## AI Integration

See MCP-SERVER-GUIDE.md for setting up Claude MCP server integration, or use:
- Text Generator plugin with Anthropic API
- Manual copy/paste to Claude.ai
- Custom scripts in vault

## Getting Started

1. Open `Dashboard.md` as your home base
2. Review example files:
   - `02-Projects/Fraud-Detection/Overview.md`
   - `01-Active/Camunda-External-Task-Research.md`
3. Create your first task using a template
4. Customize queries in Dashboard to fit your workflow

## Git Setup

```bash
git init
git add .
git commit -m "Initial vault setup"
git remote add origin git@github.com:yourusername/tech-leadership.git
git push -u origin main
```

Configure Obsidian Git plugin for auto-sync.

## Support

Questions? Check the MCP-SERVER-GUIDE.md for advanced workflows or refer to template examples in `06-Templates/`.
