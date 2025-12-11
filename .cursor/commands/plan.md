# PLAN Command - Task Planning

This command creates detailed implementation plans based on complexity level determined in VAN mode.

## Memory Bank Integration

Reads from:
- `memory-bank/tasks.md` - Task requirements and complexity level
- `memory-bank/activeContext.md` - Current project context
- `memory-bank/projectbrief.md` - Project foundation (if exists)

Updates:
- `memory-bank/tasks.md` - Adds detailed implementation plan

## Progressive Rule Loading

### Step 1: Load Core Rules
```
Load: .cursor/rules/isolation_rules/main.mdc
Load: .cursor/rules/isolation_rules/Core/memory-bank-paths.mdc
```

### Step 2: Load PLAN Mode Map
```
Load: .cursor/rules/isolation_rules/visual-maps/plan-mode-map.mdc
```

### Step 3: Load Complexity-Specific Planning Rules
Based on complexity level from `memory-bank/tasks.md`:

**Level 2:**
```
Load: .cursor/rules/isolation_rules/Level2/task-tracking-basic.mdc
Load: .cursor/rules/isolation_rules/Level2/workflow-level2.mdc
```

**Level 3:**
```
Load: .cursor/rules/isolation_rules/Level3/task-tracking-intermediate.mdc
Load: .cursor/rules/isolation_rules/Level3/planning-comprehensive.mdc
Load: .cursor/rules/isolation_rules/Level3/workflow-level3.mdc
```

**Level 4:**
```
Load: .cursor/rules/isolation_rules/Level4/task-tracking-advanced.mdc
Load: .cursor/rules/isolation_rules/Level4/architectural-planning.mdc
Load: .cursor/rules/isolation_rules/Level4/workflow-level4.mdc
```

## Workflow

1. **Read Task Context**
   - Read `memory-bank/tasks.md` to get complexity level
   - Read `memory-bank/activeContext.md` for current context
   - Review codebase structure

2. **Create Implementation Plan**
   - **Level 2:** Document planned changes, files to modify, implementation steps
   - **Level 3:** Create comprehensive plan with components, dependencies, challenges
   - **Level 4:** Create phased implementation plan with architectural considerations

3. **Technology Validation** (Level 2-4)
   - Document technology stack selection
   - Create proof of concept if needed
   - Verify dependencies and build configuration

4. **Identify Creative Phases**
   - Flag components requiring design decisions
   - Document which components need creative exploration

5. **Parallel Execution Planning** (Level 2-4)
   - **Task Decomposition**: Break down implementation plan into discrete work items
     - Analyze implementation steps
     - Identify discrete units that modify distinct sets of files
     - Create work items with unique IDs (format: `WI-[task-id]-[sequence]`)
   - **Dependency Analysis**: Identify dependencies between work items
     - File-based dependencies (work items modifying same files)
     - Functional dependencies (one provides functionality for another)
     - Data dependencies (depend on data structures created by others)
     - Integration dependencies (must integrate with each other)
   - **Parallelization Analysis**: Determine which work items can run in parallel
     - Identify work items with no dependencies
     - Detect file-level conflicts between work items
     - Create dependency graph
   - **Work Tree Assignment**: Assign work items to work trees (or mark as unassigned)
     - Check dependencies before assignment
     - Avoid conflicts with active work items
     - Balance load across available work trees
   - **Coordination Setup**: Create work item tracking structure in tasks.md
     - Document work items with status, dependencies, files affected
     - Create work tree assignment registry
     - Set up parallel execution coordination section

6. **Update Memory Bank**
   - Update `memory-bank/tasks.md` with complete plan
   - Add work items structure for parallel execution (if applicable)
   - Mark planning phase as complete

## Usage

Type `/plan` to start planning based on the task in `memory-bank/tasks.md`.

## Next Steps

- **If creative phases identified:** Use `/creative` command
- **If no creative phases:** Proceed to `/build` command
- **If work items created:** `/build` command will automatically spawn parallel agents for ready work items

