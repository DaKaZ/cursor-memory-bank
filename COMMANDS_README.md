# Memory Bank Commands

This directory contains Cursor 2.0 commands that replace the deprecated custom modes feature. Each command implements a specific phase of the Memory Bank workflow with progressive rule loading to optimize context usage.

## Available Commands

### `/van` - Initialization & Entry Point
**Purpose:** Initialize Memory Bank, detect platform, determine task complexity, and route to appropriate workflows.

**When to use:** 
- Starting a new task or project
- Initializing Memory Bank structure
- Determining task complexity

**Next steps:**
- Level 1 tasks → `/build`
- Level 2-4 tasks → `/plan`

### `/plan` - Task Planning
**Purpose:** Create detailed implementation plans based on complexity level.

**When to use:**
- After `/van` determines Level 2-4 complexity
- Need to create structured implementation plan

**Next steps:**
- Creative phases identified → `/creative`
- No creative phases → `/build`

### `/creative` - Design Decisions
**Purpose:** Perform structured design exploration for components requiring creative phases.

**When to use:**
- After `/plan` identifies components needing design decisions
- Need to explore architecture, UI/UX, or algorithm options

**Next steps:**
- After all creative phases complete → `/build`

### `/build` - Code Implementation
**Purpose:** Implement planned changes following the implementation plan and creative decisions.

**When to use:**
- After planning is complete (and creative phases if needed)
- Ready to start coding

**Next steps:**
- After implementation complete → `/reflect`

### `/reflect` - Task Reflection
**Purpose:** Facilitate structured reflection on completed implementation.

**When to use:**
- After `/build` completes implementation
- Need to document lessons learned and process improvements

**New features:**
- Automatically analyzes agent interaction history (if `.specstory/history/` exists)
- Creates interaction summaries, vibe-coaching reports, and improvement suggestions

**Next steps:**
- After reflection complete → `/archive`

### `/archive` - Task Archiving
**Purpose:** Create comprehensive archive documentation and update Memory Bank.

**When to use:**
- After `/reflect` completes reflection
- Ready to finalize task documentation

**Next steps:**
- After archiving complete → `/van` (for next task)

### `/roadmap` - Product Roadmap Management
**Purpose:** Manage product roadmap, tracking epics/features across multiple tasks.

**When to use:**
- Creating new product epics/features
- Linking tasks to roadmap items
- Viewing roadmap progress
- Updating epic/feature status

**Operations:**
- `Create epic [name]` - Create new epic/feature
- `Update status EPIC-[ID] to [status]` - Update epic status
- `Link task TASK-[ID] to EPIC-[ID]` - Link task to epic
- `Unlink task TASK-[ID] from EPIC-[ID]` - Unlink task from epic
- `View roadmap` - Display roadmap with progress
- `Update progress` - Recalculate progress for all epics

**Next steps:**
- Use `/van` to start new tasks (optionally link to roadmap)
- Use `/plan` to plan tasks with roadmap context
- Use `/archive` to update roadmap progress on task completion

## Command Workflow

```
/van → /plan → /creative → /build → /reflect → /archive
  ↓       ↓        ↓         ↓         ↓          ↓
Level 1  Level   Level    Level    Level     Level
tasks    2-4     3-4      1-4      1-4       1-4

/roadmap (can be used at any time to manage product roadmap)
```

## Progressive Rule Loading

Each command implements progressive rule loading to optimize context usage:

1. **Core Rules** - Always loaded (main.mdc, memory-bank-paths.mdc)
2. **Mode-Specific Rules** - Loaded based on command (mode maps)
3. **Complexity-Specific Rules** - Loaded based on task complexity level
4. **Specialized Rules** - Lazy loaded only when needed (e.g., creative phase types)

This approach reduces initial token usage by ~70% compared to loading all rules at once.

## Memory Bank Integration

All commands read from and update files in the `memory-bank/` directory:

- **tasks.md** - Source of truth for task tracking
- **roadmap.md** - Product roadmap with epics/features tracking
- **activeContext.md** - Current project focus
- **progress.md** - Implementation status
- **projectbrief.md** - Project foundation
- **creative/** - Creative phase documents
- **reflection/** - Reflection documents
- **archive/** - Archive documents

## Usage Examples

### Starting a New Task
```
/van Initialize project for adding user authentication feature
```

### Planning a Feature
```
/plan
```

### Exploring Design Options
```
/creative
```

### Implementing Changes
```
/build
```

### Reflecting on Completion
```
/reflect
```

### Archiving Completed Task
```
/archive
```

### Managing Product Roadmap
```
/roadmap Create epic for user authentication feature
/roadmap View roadmap
/roadmap Link task TASK-001 to EPIC-001
/roadmap Update progress
```

## Migration from Custom Modes

These commands replace the previous custom modes:
- **VAN Mode** → `/van` command
- **PLAN Mode** → `/plan` command
- **CREATIVE Mode** → `/creative` command
- **BUILD Mode** → `/build` command
- **REFLECT Mode** → `/reflect` command
- **ARCHIVE Mode** → `/archive` command

**New in v0.9:**
- **ROADMAP Mode** → `/roadmap` command (new functionality for product roadmap management)

The functionality remains the same, but now uses Cursor 2.0's commands feature instead of custom modes.

