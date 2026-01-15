---
name: bd
description: Issue tracking with bd (beads), a lightweight tracker with first-class dependency support. Use when working in projects with .beads/ directories for finding work, creating issues, updating status, managing dependencies, and syncing with git. Triggers when users mention bd/beads, ask about available work, request issue creation/updates, or at session end for sync operations.
---

# bd (beads)

Workflow guidance for bd, a lightweight issue tracker with git-based sync and dependency support.

## When to Use bd vs TodoWrite

**Use bd for**:
- Multi-session work that persists beyond current conversation
- Work discovered during development that should be tracked
- Issues with dependencies or blocking relationships
- Coordination with other developers
- Work that needs git history and audit trail

**Use TodoWrite for**:
- Simple single-session execution tracking
- Ephemeral task lists for current session only
- Breaking down immediate work into steps

**Rule of thumb**: When in doubt, prefer bd—persistence you don't need beats lost context.

## Core Workflow

### 1. Finding Work

```bash
# Show unblocked work ready to start
bd ready

# Filter by criteria
bd ready --priority=0           # Critical priority only
bd ready --label=bug            # Bugs only
bd ready --assignee=username    # Assigned to specific person
bd ready --unassigned           # Unassigned work

# View all open issues
bd list --status=open

# View your active work
bd list --status=in_progress

# View specific issue details
bd show beads-xxx
```

### 2. Creating Issues

```bash
# Basic issue creation
bd create --title="Fix login bug" --type=bug --priority=2

# With full options
bd create \
  --title="Implement user dashboard" \
  --type=feature \
  --priority=1 \
  --assignee=username \
  --label=frontend \
  --label=urgent

# Quick capture (outputs only ID for scripting)
bd q "Quick issue title"
```

**Priority format**: Use 0-4 or P0-P4 (NOT "high"/"medium"/"low")
- 0/P0: Critical
- 1/P1: High
- 2/P2: Medium (default)
- 3/P3: Low
- 4/P4: Backlog

**Issue types**: task, bug, feature, epic, merge-request (aliases: mr, feat, mol)

**Efficiency tip**: When creating multiple issues, use parallel subagents for speed.

### 3. Claiming Work

```bash
# Update status to in_progress
bd update beads-xxx --status=in_progress

# Assign to yourself
bd update beads-xxx --assignee=username

# Both at once
bd update beads-xxx --status=in_progress --assignee=username
```

### 4. Updating During Work

```bash
# Change priority
bd update beads-xxx --priority=0

# Add labels
bd update beads-xxx --label=blocked --label=needs-review

# Update assignee
bd update beads-xxx --assignee=other-user

# Add description or notes
bd edit beads-xxx  # Opens in $EDITOR
```

### 5. Completing Work

```bash
# Close single issue
bd close beads-xxx

# Close multiple issues at once (more efficient)
bd close beads-aaa beads-bbb beads-ccc

# Close with reason
bd close beads-xxx --reason="Fixed by implementing new auth flow"
```

**Important**: Always close issues when work is complete. Don't leave completed work in in_progress state.

### 6. Syncing with Git

```bash
# Full sync: pull, merge, commit, push
bd sync

# Check sync status without syncing
bd sync --status
```

**What `bd sync` does**:
1. Pull from remote (fetch + merge)
2. Merge local and remote issues (3-way merge)
3. Export merged state to JSONL
4. Commit changes to git
5. Push to remote

## Session Close Protocol

**CRITICAL**: Before ending any work session, follow this checklist:

```bash
# 1. Check what changed
git status

# 2. Stage code changes
git add <files>

# 3. Sync beads changes
bd sync

# 4. Commit code
git commit -m "Descriptive message"

# 5. Sync again (catches any new beads changes)
bd sync

# 6. Push to remote
git push

# 7. Verify everything is pushed
git status  # Should show "up to date with origin"
```

**Never skip this protocol**. Work is NOT complete until `git push` succeeds. Never say "ready to push when you are"—YOU must push.

## Dependencies

Create blocking relationships between issues:

```bash
# Add dependency (A depends on B, meaning B blocks A)
bd dep add beads-aaa beads-bbb

# View all blocked issues
bd blocked

# View what blocks/is blocked by specific issue
bd show beads-xxx
```

**Dependency direction**: `bd dep add A B` means "A depends on B" (B must complete before A).

## Quality Gates Integration

When landing work, run quality gates before syncing:

```bash
# 1. Run tests
pnpm test  # or npm test, cargo test, etc.

# 2. Run linters
pnpm lint  # or eslint, cargo clippy, etc.

# 3. Run build
pnpm build  # or npm run build, cargo build, etc.

# 4. Only if all pass, proceed with session close protocol
```

Never push failing code. If quality gates fail, fix issues and re-run.

## Common Patterns

### Discovering Work During Development

When you discover new work while implementing:

```bash
# Create issue for follow-up
bd create --title="Refactor auth middleware" --type=task --priority=3

# Add dependency if current work blocks it
bd dep add beads-new beads-current
```

### Working on Dependent Issues

```bash
# Find what's blocking this issue
bd show beads-xxx

# Complete blockers first
bd close beads-blocker

# Now the original issue appears in ready
bd ready
```

### Handling Conflicts During Sync

If `bd sync` encounters conflicts:

1. Check conflict details in output
2. Use `bd doctor` to diagnose
3. Resolve manually if needed (bd uses 3-way merge, conflicts are rare)
4. Re-run `bd sync`

If sync fails repeatedly, ask user for help—don't force-push or skip sync.

## Advanced Features

For advanced features like epics, molecules, gates, swarms, and bulk operations, see [references/advanced.md](references/advanced.md).

Quick summary of when to load advanced.md:
- **Epics**: Grouping related issues hierarchically
- **Molecules**: Work templates with coordinated steps
- **Gates**: Async coordination points
- **Swarms**: Parallel task groups
- **Merge slots**: Serializing conflict-prone work
- **Bulk operations**: Operating on multiple issues efficiently

## Quick Reference

```bash
bd ready                              # Find available work
bd show <id>                          # View issue details
bd create "Title" --type task --priority 2  # Create issue
bd update <id> --status=in_progress   # Claim work
bd close <id1> <id2> ...              # Complete work (multiple at once)
bd dep add <issue> <depends-on>       # Add dependency
bd blocked                            # Show blocked issues
bd sync                               # Sync with git
bd doctor                             # Check system health
```

## Troubleshooting

```bash
# Check system health
bd doctor

# View daemon and database info
bd info

# Check if hooks are installed
bd hooks list

# Install hooks if missing
bd hooks install

# Repair corrupted database
bd repair
```

If `bd doctor` shows issues, address them before continuing work.
