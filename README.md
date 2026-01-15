# BD Skill for Claude Code

A Claude Code skill for issue tracking with [bd (beads)](https://github.com/bd-labs/bd), a lightweight issue tracker with first-class dependency support and git-based synchronization.

## What is this?

This repository contains a Claude Code skill that enables Claude to work seamlessly with bd for issue tracking and project management. The skill provides workflow guidance, command patterns, and best practices for using bd in development sessions.

## What is bd?

bd (beads) is a lightweight, developer-friendly issue tracker that:
- Lives alongside your code in `.beads/` directories
- Syncs with git for distributed collaboration
- Supports dependencies and blocking relationships between issues
- Provides a simple CLI for fast issue management
- Integrates naturally with development workflows

## Installation

### Prerequisites

1. Install bd: Follow the [bd installation guide](https://github.com/bd-labs/bd)
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

- [bd (beads)](https://github.com/bd-labs/bd) - The issue tracker this skill integrates with
- [Claude Code](https://github.com/anthropics/claude-code) - Anthropic's official CLI for Claude
