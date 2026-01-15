# Advanced bd Features

This document covers advanced bd features beyond the core workflow. Load this when working with epics, molecules, gates, or other advanced dependency structures.

## Epics

Epics group related issues hierarchically:

```bash
# Create epic
bd create --title="User Authentication" --type=epic

# Create child issues
bd create --title="Login form" --type=task --parent=beads-xxx

# View epic tree
bd show beads-xxx
```

## Dependencies

Dependencies create blocking relationships between issues:

```bash
# Add dependency (A depends on B, meaning B blocks A)
bd dep add beads-aaa beads-bbb

# Remove dependency
bd dep remove beads-aaa beads-bbb

# View all blocked issues
bd blocked

# View what blocks/is blocked by specific issue
bd show beads-xxx
```

**Dependency direction**: `bd dep add A B` means "A depends on B" (B must complete before A can start).

## Molecules

Molecules are work templates with steps and gates for coordination:

```bash
# List available molecule types
bd mol list-types

# Create molecule instance
bd mol create --type=patrol --title="Weekly security patrol"

# View molecule steps
bd ready --mol beads-xxx

# Work on specific step
bd update beads-step-xxx --status=in_progress
```

### Gates

Gates coordinate async work within molecules:

```bash
# View gate status
bd gate status beads-xxx

# Close gate (signal completion)
bd gate close beads-xxx

# Find molecules ready for gate-resume
bd ready --gated
```

## Swarms

Swarms are structured epics with parallel task groups:

```bash
# Create swarm
bd swarm create --title="API Migration" --steps=3

# Swarm automatically creates structure with gates between phases
```

## Merge Slots

Merge slots serialize conflict-prone work (e.g., database migrations):

```bash
# Create merge-slot gate
bd merge-slot create --title="DB Schema Changes"

# Issues waiting for slot
bd merge-slot show beads-xxx

# Grant slot to next issue
bd merge-slot grant beads-xxx
```

## Advanced Filtering

```bash
# Filter by multiple criteria
bd list --status=open --priority=0 --label=bug --assignee=username

# Filter by parent (show all descendants)
bd ready --parent=beads-xxx

# Filter by molecule type
bd ready --mol-type=patrol

# Include deferred issues
bd ready --include-deferred
```

## Bulk Operations

```bash
# Close multiple issues at once (efficient)
bd close beads-aaa beads-bbb beads-ccc

# Update multiple issues
bd update beads-aaa beads-bbb --priority=1

# Delete multiple issues
bd delete beads-aaa beads-bbb
```

## Issue States

Beyond open/in_progress/closed:

```bash
# Defer issue for later
bd defer beads-xxx --until="2026-02-01"

# Undefer issue
bd undefer beads-xxx

# Mark as duplicate
bd duplicate beads-xxx beads-yyy  # xxx is duplicate of yyy

# Mark as superseded
bd supersede beads-xxx beads-yyy  # xxx superseded by yyy
```

## Search and Discovery

```bash
# Text search across titles and descriptions
bd search "authentication bug"

# Find stale issues (not updated recently)
bd stale --days=30

# Count issues matching filters
bd count --status=open --label=bug
```

## Collaboration

```bash
# Assign to team member
bd update beads-xxx --assignee=username

# Add comments
bd comments add beads-xxx "Design review feedback: ..."

# View comments
bd comments show beads-xxx

# View activity feed
bd activity
```

## Sync Variations

```bash
# Check sync status without syncing
bd sync --status

# Dry run (preview changes)
bd sync --dry-run

# Skip pull (just export, commit, push)
bd sync --no-pull

# Skip push (just pull and merge)
bd sync --no-push

# Import only (after manual git pull)
bd sync --import-only

# Flush only (export to JSONL without git ops)
bd sync --flush-only
```

## Troubleshooting

```bash
# Check system health
bd doctor

# Repair corrupted database
bd repair

# Check daemon status
bd info

# Restart daemon
# (Kill daemon process and it will auto-restart on next bd command)
```
