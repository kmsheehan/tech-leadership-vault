# Obsidian MCP Server Guide

Complete guide to integrating Claude with your Obsidian vault using Model Context Protocol (MCP).

## What is MCP?

Model Context Protocol (MCP) is Anthropic's standard for connecting AI models to external data sources and tools. An MCP server exposes your Obsidian vault to Claude, allowing it to:

- Read notes and understand context from multiple files
- Create new notes directly in your vault
- Update existing notes
- Query vault content based on tags, folders, metadata
- Generate content that follows your templates and patterns

## Architecture

```
┌─────────────────┐
│  Claude Desktop │
│   or Claude.ai  │
└────────┬────────┘
         │ MCP Protocol
         │
┌────────▼────────────────┐
│  Obsidian MCP Server    │
│  (Node.js/TypeScript)   │
└────────┬────────────────┘
         │ File System Access
         │
┌────────▼────────────────┐
│   Your Obsidian Vault   │
│  /path/to/vault/        │
└─────────────────────────┘
```

## Benefits Over Other AI Integration Methods

| Feature | Text Generator Plugin | Manual Copy/Paste | MCP Server |
|---------|----------------------|-------------------|------------|
| Multi-file context | ❌ | Limited | ✅ |
| Direct file creation | ✅ | ❌ | ✅ |
| Template awareness | Limited | ❌ | ✅ |
| Vault queries | ❌ | ❌ | ✅ |
| Workflow automation | Limited | ❌ | ✅ |
| Custom logic | ❌ | ❌ | ✅ |

## Implementation Options

### Option 1: Use Existing MCP Server (Easiest)

There are open-source Obsidian MCP servers available. Search for "obsidian-mcp" on GitHub.

**Setup:**
1. Clone the repository
2. Configure with your vault path
3. Add to Claude Desktop config
4. Start using with Claude

### Option 2: Build Custom MCP Server (Most Flexible)

Build your own tailored to your workflow. See implementation guide below.

## Custom MCP Server Implementation

### Prerequisites

```bash
# Node.js 18+
node --version

# TypeScript
npm install -g typescript

# MCP SDK
npm install @anthropic-ai/sdk
```

### Basic Server Structure

**File: `obsidian-mcp-server/package.json`**
```json
{
  "name": "obsidian-mcp-server",
  "version": "1.0.0",
  "type": "module",
  "main": "dist/index.js",
  "scripts": {
    "build": "tsc",
    "start": "node dist/index.js"
  },
  "dependencies": {
    "@anthropic-ai/sdk": "^0.14.0",
    "gray-matter": "^4.0.3",
    "glob": "^10.3.10"
  },
  "devDependencies": {
    "@types/node": "^20.10.0",
    "typescript": "^5.3.0"
  }
}
```

**File: `obsidian-mcp-server/tsconfig.json`**
```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "ES2022",
    "moduleResolution": "node",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules"]
}
```

**File: `obsidian-mcp-server/src/index.ts`**
```typescript
import { Server } from '@anthropic-ai/sdk';
import { readFileSync, writeFileSync, readdirSync, statSync } from 'fs';
import { join, relative } from 'path';
import matter from 'gray-matter';
import { glob } from 'glob';

const VAULT_PATH = process.env.OBSIDIAN_VAULT_PATH || '/path/to/tech-leadership-vault';

interface Note {
  path: string;
  name: string;
  content: string;
  frontmatter: any;
  tags: string[];
  links: string[];
}

class ObsidianMCPServer {
  private vaultPath: string;

  constructor(vaultPath: string) {
    this.vaultPath = vaultPath;
  }

  // Read a single note
  readNote(relativePath: string): Note {
    const fullPath = join(this.vaultPath, relativePath);
    const content = readFileSync(fullPath, 'utf-8');
    const { data, content: body } = matter(content);
    
    // Extract tags from frontmatter and content
    const tags = [
      ...(data.tags || []),
      ...this.extractTagsFromContent(body)
    ];
    
    // Extract wikilinks
    const links = this.extractLinks(body);
    
    return {
      path: relativePath,
      name: relativePath.replace(/\.md$/, ''),
      content: body,
      frontmatter: data,
      tags,
      links
    };
  }

  // Search notes by query
  searchNotes(query: {
    tags?: string[];
    folder?: string;
    search?: string;
  }): Note[] {
    const pattern = query.folder 
      ? join(this.vaultPath, query.folder, '**/*.md')
      : join(this.vaultPath, '**/*.md');
    
    const files = glob.sync(pattern);
    let notes = files.map(f => this.readNote(relative(this.vaultPath, f)));
    
    // Filter by tags
    if (query.tags && query.tags.length > 0) {
      notes = notes.filter(n => 
        query.tags!.some(tag => n.tags.includes(tag))
      );
    }
    
    // Filter by search text
    if (query.search) {
      const searchLower = query.search.toLowerCase();
      notes = notes.filter(n => 
        n.content.toLowerCase().includes(searchLower) ||
        n.name.toLowerCase().includes(searchLower)
      );
    }
    
    return notes;
  }

  // Create new note
  createNote(relativePath: string, content: string, frontmatter?: any): void {
    const fullPath = join(this.vaultPath, relativePath);
    
    let fileContent = content;
    if (frontmatter) {
      fileContent = matter.stringify(content, frontmatter);
    }
    
    writeFileSync(fullPath, fileContent, 'utf-8');
  }

  // Update existing note
  updateNote(relativePath: string, content: string, mergeFrontmatter?: any): void {
    const note = this.readNote(relativePath);
    const newFrontmatter = mergeFrontmatter 
      ? { ...note.frontmatter, ...mergeFrontmatter }
      : note.frontmatter;
    
    this.createNote(relativePath, content, newFrontmatter);
  }

  // Get tasks from vault
  getTasks(filter?: { status?: string; project?: string }): any[] {
    const notes = this.searchNotes({ folder: '01-Active' });
    let tasks = notes.filter(n => n.frontmatter.status);
    
    if (filter?.status) {
      tasks = tasks.filter(n => n.frontmatter.status === filter.status);
    }
    
    if (filter?.project) {
      tasks = tasks.filter(n => n.frontmatter.project?.includes(filter.project));
    }
    
    return tasks.map(t => ({
      name: t.name,
      path: t.path,
      status: t.frontmatter.status,
      priority: t.frontmatter.priority,
      project: t.frontmatter.project
    }));
  }

  // Get content pipeline status
  getContentPipeline(): any {
    const linkedin = this.searchNotes({ folder: '04-Content/LinkedIn' });
    const internal = this.searchNotes({ folder: '04-Content/Internal' });
    
    return {
      linkedin: {
        drafts: linkedin.filter(n => n.frontmatter.status === 'draft').length,
        ready: linkedin.filter(n => n.frontmatter.status === 'ready').length,
        published: linkedin.filter(n => n.frontmatter.status === 'published').length
      },
      internal: {
        drafts: internal.filter(n => n.frontmatter.status === 'draft').length,
        published: internal.filter(n => n.frontmatter.status === 'published').length
      }
    };
  }

  private extractTagsFromContent(content: string): string[] {
    const tagRegex = /#([a-zA-Z0-9_/-]+)/g;
    const matches = content.matchAll(tagRegex);
    return Array.from(matches, m => m[1]);
  }

  private extractLinks(content: string): string[] {
    const linkRegex = /\[\[([^\]]+)\]\]/g;
    const matches = content.matchAll(linkRegex);
    return Array.from(matches, m => m[1]);
  }
}

// MCP Server setup
const server = new Server({
  name: 'obsidian-vault',
  version: '1.0.0'
});

const obsidian = new ObsidianMCPServer(VAULT_PATH);

// Register tools
server.tool(
  'read_note',
  'Read a single note from the vault',
  {
    path: { type: 'string', description: 'Relative path to note (e.g., "02-Projects/Fraud-Detection/Overview.md")' }
  },
  async ({ path }) => {
    const note = obsidian.readNote(path);
    return {
      content: note.content,
      frontmatter: note.frontmatter,
      tags: note.tags,
      links: note.links
    };
  }
);

server.tool(
  'search_notes',
  'Search notes by tags, folder, or text',
  {
    tags: { type: 'array', items: { type: 'string' }, optional: true },
    folder: { type: 'string', optional: true },
    search: { type: 'string', optional: true }
  },
  async (params) => {
    const notes = obsidian.searchNotes(params);
    return notes.map(n => ({
      path: n.path,
      name: n.name,
      tags: n.tags,
      preview: n.content.substring(0, 200) + '...'
    }));
  }
);

server.tool(
  'create_note',
  'Create a new note in the vault',
  {
    path: { type: 'string', description: 'Relative path including filename' },
    content: { type: 'string', description: 'Note content' },
    frontmatter: { type: 'object', optional: true, description: 'YAML frontmatter' }
  },
  async ({ path, content, frontmatter }) => {
    obsidian.createNote(path, content, frontmatter);
    return { success: true, path };
  }
);

server.tool(
  'update_note',
  'Update an existing note',
  {
    path: { type: 'string' },
    content: { type: 'string' },
    frontmatter: { type: 'object', optional: true }
  },
  async ({ path, content, frontmatter }) => {
    obsidian.updateNote(path, content, frontmatter);
    return { success: true, path };
  }
);

server.tool(
  'get_tasks',
  'Get tasks from kanban board',
  {
    status: { type: 'string', optional: true, enum: ['backlog', 'todo', 'doing', 'review', 'done'] },
    project: { type: 'string', optional: true }
  },
  async (params) => {
    return obsidian.getTasks(params);
  }
);

server.tool(
  'get_content_pipeline',
  'Get content pipeline status',
  {},
  async () => {
    return obsidian.getContentPipeline();
  }
);

// Start server
console.log(`Starting Obsidian MCP Server...`);
console.log(`Vault path: ${VAULT_PATH}`);
server.listen();
```

### Build and Run

```bash
cd obsidian-mcp-server
npm install
npm run build
OBSIDIAN_VAULT_PATH=/path/to/tech-leadership-vault npm start
```

## Configure Claude Desktop

**File: `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS)**
**File: `%APPDATA%\Claude\claude_desktop_config.json` (Windows)**

```json
{
  "mcpServers": {
    "obsidian": {
      "command": "node",
      "args": ["/path/to/obsidian-mcp-server/dist/index.js"],
      "env": {
        "OBSIDIAN_VAULT_PATH": "/path/to/tech-leadership-vault"
      }
    }
  }
}
```

Restart Claude Desktop to load the MCP server.

## Usage Examples

### Example 1: Create Task from Conversation

**You:** "I need to research Camunda retry patterns for our fraud detection work"

**Claude:** *Uses MCP tools*
```
<use_tool>
<tool_name>create_note</tool_name>
<parameters>
<path>01-Active/Research-Camunda-Retry-Patterns.md</path>
<content>
## Description
Research Camunda retry patterns for AI service timeouts in fraud detection workflow.

## Acceptance Criteria
- [ ] Document retry configuration options
- [ ] Test with Bedrock timeouts
- [ ] Create example code
</content>
<frontmatter>{
  "status": "todo",
  "priority": "high",
  "project": "Fraud-Detection"
}</frontmatter>
</parameters>
</use_tool>
```

### Example 2: Generate Content from Multiple Notes

**You:** "Create a LinkedIn post about the Camunda + AI work, pulling from my recent learning and project notes"

**Claude:** *Reads context*
```
<use_tool>
<tool_name>search_notes</tool_name>
<parameters>
<tags>["learning", "camunda"]</tags>
</parameters>
</use_tool>

<use_tool>
<tool_name>read_note</tool_name>
<parameters>
<path>02-Projects/Fraud-Detection/Overview.md</path>
</parameters>
</use_tool>
```

*Then creates post with full context*
```
<use_tool>
<tool_name>create_note</tool_name>
<parameters>
<path>04-Content/LinkedIn/When-Workflows-Meet-Agents.md</path>
<content>[Generated content based on your notes]</content>
<frontmatter>{
  "status": "draft",
  "theme": "workflow-orchestration",
  "target-audience": "technical-architects"
}</frontmatter>
</parameters>
</use_tool>
```

### Example 3: Daily Standup Summary

**You:** "What am I working on today?"

**Claude:** *Queries your tasks*
```
<use_tool>
<tool_name>get_tasks</tool_name>
<parameters>
<status>doing</status>
</parameters>
</use_tool>
```

Returns your active tasks with context.

### Example 4: Content Pipeline Status

**You:** "What's my content pipeline look like?"

**Claude:**
```
<use_tool>
<tool_name>get_content_pipeline</tool_name>
</use_tool>
```

Shows drafts, ready-to-publish, published counts.

## Advanced Workflows

### Workflow 1: Weekly Review
Claude can automatically:
1. Query tasks completed this week
2. Gather learning notes
3. Check content pipeline
4. Generate summary report
5. Suggest next week's priorities

### Workflow 2: Content Generation
Claude can:
1. Read project notes for context
2. Pull related learning
3. Review similar past posts
4. Generate draft following your template
5. Save to content pipeline with proper metadata

### Workflow 3: Pattern Extraction
Claude can:
1. Review multiple project notes
2. Identify common patterns
3. Generate architecture pattern doc
4. Link to relevant projects
5. Suggest internal/external content opportunities

## Security Considerations

1. **File System Access**: MCP server has full access to vault
2. **API Keys**: Never store API keys in vault files
3. **Sensitive Data**: Be cautious about what Claude can read
4. **Git Commits**: Review before pushing

## Troubleshooting

### Server Won't Start
- Check Node.js version (18+)
- Verify vault path exists
- Check for port conflicts

### Claude Can't See Server
- Restart Claude Desktop
- Check config file syntax
- Verify server is running (`ps aux | grep obsidian-mcp`)

### Permission Errors
- Ensure vault directory has read/write permissions
- Check .obsidian folder isn't corrupted

## Next Steps

1. **Start Simple**: Use manual workflow first
2. **Test MCP**: Try with example server
3. **Customize**: Build workflows specific to your needs
4. **Iterate**: Refine based on usage

## Alternative: Simpler Script Approach

If full MCP server is too complex, create simple Node scripts:

**File: `scripts/create-task.js`**
```javascript
// Simple script to create task from command line
// Usage: node scripts/create-task.js "Task title" "project-name"
```

**File: `scripts/query-tasks.js`**
```javascript
// Query and display tasks
// Usage: node scripts/query-tasks.js --status=doing
```

Call these from Obsidian using shell commands plugin, or from external tools.

## Resources

- MCP Documentation: https://modelcontextprotocol.io/
- Anthropic MCP SDK: https://github.com/anthropics/anthropic-sdk-typescript
- Obsidian API: https://docs.obsidian.md/

---

**Ready to integrate?** Start with the basic server implementation and expand based on your workflows.
