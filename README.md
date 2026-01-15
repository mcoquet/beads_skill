# BD Skill for Claude Code

A Claude Code skill for issue tracking with [bd (beads)](https://github.com/steveyegge/beads), a distributed, git-backed graph issue tracker designed specifically for AI coding agents.

## What is this?

This repository contains a Claude Code skill that enables Claude to work seamlessly with bd for issue tracking and project management. The skill provides workflow guidance, command patterns, and best practices for using bd in development sessions.

## What is bd?

bd (beads), created by [Steve Yegge](https://github.com/steveyegge), is a distributed issue tracker designed for AI coding agents that:
- Stores issues as JSONL files in `.beads/` directories alongside your code
- Provides git-based version control for distributed collaboration
- Supports dependency tracking with blocking relationships and hierarchical structures
- Uses hash-based IDs (like `bd-a1b2`) to prevent conflicts in multi-agent workflows
- Includes semantic summarization of completed tasks to preserve context
- Optimized for agents with JSON output and automatic detection of ready-to-work tasks

## Installation

### Prerequisites

1. **Install bd** using one of these methods:
   ```bash
   # npm
   npm install -g @beads/bd

   # Homebrew
   brew install steveyegge/beads/bd

   # Go
   go install github.com/steveyegge/beads/cmd/bd@latest
   ```

2. Have Claude Code installed and configured

### Installing the Skill

1. Download the skill file:
   ```bash
   curl -LO https://github.com/mcoquet/beads_skill/raw/main/bd.skill
   ```

2. Install it to Claude Code's skills directory:
   ```bash
   # Copy to your skills directory (adjust path as needed)
   cp bd.skill ~/.claude/skills/
   ```

3. Verify installation by running Claude Code and checking available skills

## Features

The bd skill enables Claude to:

- **Find available work**: Query for unblocked, ready-to-start issues
- **Create issues**: Generate well-structured tasks, bugs, and features
- **Manage status**: Update issue status and assignments during development
- **Handle dependencies**: Create and manage blocking relationships between issues
- **Sync with git**: Coordinate bd state across team members
- **Follow best practices**: Ensure proper session close protocols and quality gates

## Usage

Once installed, Claude will automatically use the bd skill when:
- You mention bd or beads in your requests
- You ask about available work
- You request issue creation or updates
- At the end of sessions (for proper sync)

Example interactions:
```
"What work is available in bd?"
"Create a bug for the login timeout issue"
"Update beads-123 to in progress"
"Show me what's blocking beads-456"
```

## When to Use bd vs TodoWrite

The skill helps Claude decide between bd and TodoWrite:

**Use bd for**:
- Multi-session work that persists beyond current conversation
- Work discovered during development that should be tracked
- Issues with dependencies or blocking relationships
- Coordination with other developers

**Use TodoWrite for**:
- Simple single-session execution tracking
- Ephemeral task lists for current session only
- Breaking down immediate work into steps

## Documentation

- [SKILL.md](bd/SKILL.md) - Complete skill documentation and workflow guide
- [advanced.md](bd/references/advanced.md) - Advanced features (epics, molecules, gates, swarms)

## Repository Contents

```
.
├── README.md                   # This file
├── bd.skill                    # Packaged skill file (zip format)
└── bd/                         # Unzipped skill contents
    ├── SKILL.md                # Main skill documentation
    └── references/
        └── advanced.md         # Advanced features reference
```

## Contributing

Issues and pull requests are welcome! If you find bugs or have suggestions for improving the skill, please open an issue.

## License

This skill is provided as-is for use with Claude Code and bd.

## Related Projects

- [bd (beads)](https://github.com/steveyegge/beads) - The issue tracker this skill integrates with
- [Claude Code](https://github.com/anthropics/claude-code) - Anthropic's official CLI for Claude
