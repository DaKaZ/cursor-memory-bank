# ROADMAP Command - Product Roadmap Management

This command manages the product roadmap, tracking high-level product goals (epics/features) that span multiple tasks.

## Memory Bank Integration

**CRITICAL:** All Memory Bank files are located in `memory-bank/` directory:
- `memory-bank/roadmap.md` - Product roadmap with epics/features tracking
- `memory-bank/tasks.md` - Task tracking (for linking tasks to roadmap items)

Reads from:
- `memory-bank/roadmap.md` - Existing roadmap structure
- `memory-bank/tasks.md` - Task information for linking

Updates:
- `memory-bank/roadmap.md` - Roadmap structure and progress
- `memory-bank/tasks.md` - Task roadmap links (when linking/unlinking)

## Progressive Rule Loading

This command loads rules progressively to optimize context usage:

### Step 1: Load Core Rules (Always Required)
```
Load: .cursor/rules/isolation_rules/main.mdc
Load: .cursor/rules/isolation_rules/Core/memory-bank-paths.mdc
Load: .cursor/rules/isolation_rules/Core/roadmap-management.mdc
```

### Step 2: Load ROADMAP Mode Map
```
Load: .cursor/rules/isolation_rules/visual-maps/roadmap-mode-map.mdc
```

## Workflow

1. **Check Roadmap File**
   - Check if `memory-bank/roadmap.md` exists
   - If not, create with template structure
   - If exists, read current roadmap

2. **Determine Operation**
   Based on user request, perform one of:
   - **Create Epic/Feature**: Add new epic/feature to roadmap
   - **Update Status**: Change epic/feature status
   - **Link Task**: Link a task to an epic/feature
   - **Unlink Task**: Remove task link from epic/feature
   - **View Roadmap**: Display roadmap with current progress
   - **Update Progress**: Recalculate progress metrics

3. **Create Epic/Feature**
   - Get epic details: name, description, priority, target date, dependencies
   - Generate epic ID (format: `EPIC-[sequential]`)
   - Create epic structure with status "Planned"
   - Save to `roadmap.md`

4. **Update Status**
   - Select epic/feature to update
   - Get new status (Planned | In Progress | Blocked | Completed)
   - Validate status transition
   - Update epic status in `roadmap.md`

5. **Link Task**
   - Select epic/feature to link to
   - Get task ID from `tasks.md`
   - Verify task exists
   - Add task to epic's linked tasks list
   - Update `tasks.md` with roadmap link field
   - Recalculate epic progress

6. **Unlink Task**
   - Select epic/feature
   - Select task to unlink
   - Remove task from epic's linked tasks list
   - Remove roadmap link from `tasks.md`
   - Recalculate epic progress

7. **View Roadmap**
   - Display all epics/features with:
     - Status summary
     - Progress metrics
     - Linked tasks
     - Dependencies

8. **Update Progress**
   - Read all linked tasks from `tasks.md`
   - Calculate progress metrics:
     - Completed tasks count
     - Total tasks count
     - Progress percentage
   - Update epic progress in `roadmap.md`
   - Check if all tasks complete
   - Auto-update status to "Completed" if all tasks done

## Usage

Type `/roadmap` followed by the operation you want to perform.

Examples:
```
/roadmap Create epic for user authentication feature
/roadmap Update status EPIC-001 to In Progress
/roadmap Link task TASK-001 to EPIC-001
/roadmap View roadmap
/roadmap Update progress for all epics
```

## Roadmap File Structure

The roadmap file follows this structure:

```markdown
# Product Roadmap

## Epics/Features

### EPIC-001: [Epic Name]
- **Status**: Planned | In Progress | Blocked | Completed
- **Priority**: High | Medium | Low
- **Target Date**: YYYY-MM-DD (optional)
- **Progress**: X% (Y tasks completed / Z total tasks)
- **Description**: [Epic description]
- **Linked Tasks**: 
  - TASK-001: [Task Name] (Status)
  - TASK-002: [Task Name] (Status)
- **Dependencies**: [Other epic IDs or "None"]
- **Notes**: [Additional context]
```

## Integration with Other Commands

- **`/van`**: Optionally links new tasks to roadmap items during initialization
- **`/plan`**: Reads roadmap context when task is linked to epic/feature
- **`/archive`**: Updates roadmap progress when tasks complete

## Next Steps

After managing roadmap:
- Use `/van` to start new tasks (optionally link to roadmap)
- Use `/plan` to plan tasks with roadmap context
- Use `/archive` to update roadmap progress on task completion

