# MCP TaskManager

Model Context Protocol server for Task Management. This allows Claude Desktop (or any MCP client) to manage and execute tasks in a queue-based system.

## Prerequisites

- Node.js 18+ (install via `brew install node`)
- Claude Desktop (install from https://claude.ai/desktop)
- tsx (install via `npm install -g tsx`)

## Installation

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/pashpashpash/mcp-taskmanager.git
   cd mcp-taskmanager
   ```

2. **Install Dependencies**:
   ```bash
   npm install
   ```

3. **Build the Project**:
   ```bash
   npm run build
   ```

4. **Configure Claude Desktop**:

Locate your Claude Desktop configuration file at:
- macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
- Windows: `%APPDATA%/Claude/claude_desktop_config.json`

You can also find this through the Claude Desktop menu:
1. Open Claude Desktop
2. Click Claude on the Mac menu bar
3. Click "Settings"
4. Click "Developer"

Add the following to your configuration:
```json
{
  "tools": {
    "taskmanager": {
      "command": "node",
      "args": ["path/to/mcp-taskmanager/dist/index.js"]
    }
  }
}
```
Note: Replace "path/to/mcp-taskmanager" with the actual path to your cloned repository.

## Development Setup

1. **Install tsx globally** (if not already installed):
   ```bash
   npm install -g tsx
   ```

2. **Development Configuration**:
   
   For development with the TypeScript source, modify your Claude Desktop config:
   ```json
   {
     "tools": {
       "taskmanager": {
         "command": "tsx",
         "args": ["path/to/mcp-taskmanager/index.ts"]
       }
     }
   }
   ```

## Available Operations

The TaskManager supports two main phases of operation:

### Planning Phase
- Accepts a task list (array of strings) from the user
- Stores tasks internally as a queue
- Returns an execution plan (task overview, task ID, current queue status)

### Execution Phase
- Returns the next task from the queue when requested
- Provides feedback mechanism for task completion
- Removes completed tasks from the queue
- Prepares the next task for execution

### Parameters
- `action`: "plan" | "execute" | "complete"
- `tasks`: Array of task strings (required for "plan" action)
- `taskId`: Task identifier (required for "complete" action)
- `getNext`: Boolean flag to request next task (for "execute" action)

## Example Usage

```typescript
// Planning phase
{
  action: "plan",
  tasks: ["Task 1", "Task 2", "Task 3"]
}

// Execution phase
{
  action: "execute",
  getNext: true
}

// Complete task
{
  action: "complete",
  taskId: "task-123"
}
```

## Debugging

If you run into issues, check Claude Desktop's MCP logs:
```bash
tail -n 20 -f ~/Library/Logs/Claude/mcp*.log
```

## Development

```bash
# Install dependencies
npm install

# Build the project
npm run build

# Development with auto-rebuild
npm run watch
```

## License

MIT

---
Note: This is a fork of the [original mcp-taskmanager repository](https://github.com/kazuph/mcp-taskmanager).
