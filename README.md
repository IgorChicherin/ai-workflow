# Global OpenCode Configuration

This repository contains a global OpenCode configuration that provides a multi-agent orchestration workflow for all projects. The configuration installs to your global OpenCode config directory and applies automatically to every project you work on.

## Installation

1. Copy the contents of this repository to your global OpenCode config directory:
   - **Linux/macOS**: `~/.config/opencode/`
   - **Windows**: `%APPDATA%\opencode\`

2. Install the required plugin:
   ```bash
   opencode plugin install github:beremaran/opencode-agent-tree
   ```

3. Restart OpenCode.

## Agent Roster

| Agent | Model | Purpose |
|-------|-------|---------|
| **Manager** | anthropic/claude-sonnet-4-6 | Orchestrator: decomposes tasks, delegates to workers, verifies results, escalates when needed |
| **SimpleWorker** | opencode/nemotron-3-ultra-free | Simple functions, types, DTOs, interfaces, straightforward CRUD, boilerplate, simple tests, one-file fixes |
| **MediumWorker** | opencode/big-pickle | Medium features, multi-file implementation, integrations, migrations, moderate debugging |
| **ComplexWorker** | anthropic/claude-sonnet-4-6 | Difficult debugging, complex algorithms, concurrency, security-sensitive work, failed simpler worker tasks |
| **Reviewer** | anthropic/claude-sonnet-4-6 | Code review: correctness, regressions, edge cases, security, test verification |
| **Architect** | anthropic/claude-opus-4-8 | System architecture, major refactoring strategy, subsystem design, difficult trade-offs |

## How It Works

The **Manager** agent is the primary entry point. When you give it a task, it:

1. **Decomposes** the work into independent subtasks
2. **Routes** each subtask to the appropriate worker based on complexity:
   - SimpleWorker for boilerplate, types, simple functions
   - MediumWorker for multi-file features and integrations
   - ComplexWorker for difficult debugging and algorithms
   - Architect for architectural decisions
3. **Parallelizes** independent subtasks when possible
4. **Escalates** if a worker fails twice with the same approach (SimpleWorker → MediumWorker → ComplexWorker → Architect)
5. **Reviews** completed work via the Reviewer agent before considering the task done

The `opencode-agent-tree` plugin enables this multi-agent orchestration, running agents as isolated subagents with restricted tool access for safety.

## File Structure

```
opencode/
├── opencode.jsonc          # Main configuration
├── agents/
│   ├── Manager.md          # Orchestrator agent
│   ├── SimpleWorker.md     # Simple task worker
│   ├── MediumWorker.md     # Medium task worker
│   ├── ComplexWorker.md    # Complex task worker
│   ├── Reviewer.md         # Code review agent
│   └── Architect.md        # Architecture agent
└── AGENTS.md               # Global engineering rules (also copied to project roots)
```

## Global Rules

The `AGENTS.md` file contains engineering rules that apply globally. Place it in each project root to give agents consistent behavior across workspaces.