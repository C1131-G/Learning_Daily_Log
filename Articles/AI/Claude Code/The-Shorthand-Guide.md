# The Shorthand Guide to Everything Claude Code

## 🎯 Core Concepts

### 🤖 What is Claude Code?
Claude Code is a powerful AI coding assistant that integrates deeply with your terminal and development workflow. It goes beyond simple autocomplete by offering skills, hooks, and subagent orchestration.

---

## 📝 Key Features & Workflows

### 🛠️ Skills & Commands
- **Skills**: Shorthand prompts for specific, repeatable workflows (e.g., `/refactor-clean`, `/tdd`).
- **Commands**: Executable prompts stored in `~/.claude/commands`, allowing you to build a library of custom AI actions.

### 🪝 Hooks
Trigger-based automations that run around tool use:
- **PreToolUse**: Run checks before an action (e.g., "Remind me to use `tmux` for long tasks").
- **PostToolUse**: Auto-cleanup after an action (e.g., "Run `prettier` after any file edit").

### 🕵️ Subagents
Orchestrator-led processes for complex tasks. instead of one big prompt, use specialized "agents":
- `planner.md`: Breaks down the problem.
- `architect.md`: Designs the solution.
- `coder.md`: Implements the code.

### 🧠 Rules & Memory
- **`.rules` folders / `CLAUDE.md`**: Define persistent best practices for your project (e.g., "No emojis in comments", "Always use Typescript").
- **Memory**: Claude learns from your corrections over time if configured correctly.

### 🔌 MCPs (Model Context Protocol)
Connect Claude to external tools and data sources:
- **Integrations**: Supabase, GitHub, local files, etc.
- **Best Practice**: Keep enabled MCPs under 10 per project to avoid cluttering the context window.

### 🧩 Plugins & Tips
- **Plugins**: Bundles like `mgrep` (search) or `hookify` (automation).
- **Hotkeys**: `Ctrl+U` (delete line), `!` (bash prefix), `@` (file search).
- **Parallelism**: Use `/fork` and `git worktree` to let Claude work on multiple branches simultaneously.
- **Monitoring**: Use `tmux` to keep an eye on background AI processes.

---

## 💡 Key Takeaways

1.  **Customize Your Assistant**: Don't just use default prompts. Build a library of Skills and Commands that match *your* coding style.
2.  **Automate Governance**: Use Hooks to enforce quality (linting, formatting) automatically, so you don't have to nitpick the AI.
3.  **Divide and Conquer**: For large features, use Subagents (Planner -> Architect -> Coder) rather than trying to do it all in one context.
4.  **Context Hygiene**: Manage your MCPs and context window carefully to keep Claude smart and focused.
