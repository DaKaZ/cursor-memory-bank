# BUILD Command - Code Implementation

This command implements the planned changes following the implementation plan and creative phase decisions. It enforces a test-driven approach where tests are written for all success criteria and must pass before completing each phase.

## Memory Bank Integration

Reads from:
- `memory-bank/tasks.md` - Implementation plan and checklists
- `memory-bank/creative/creative-*.md` - Design decisions (Level 3-4)
- `memory-bank/activeContext.md` - Current project context

Updates:
- `memory-bank/tasks.md` - Implementation progress, test results, and status
- `memory-bank/progress.md` - Build status, test outcomes, and observations

## Progressive Rule Loading

### Step 1: Load Core Rules
```
Load: .cursor/rules/isolation_rules/main.mdc
Load: .cursor/rules/isolation_rules/Core/memory-bank-paths.mdc
Load: .cursor/rules/isolation_rules/Core/command-execution.mdc
```

### Step 2: Load BUILD Mode Map
```
Load: .cursor/rules/isolation_rules/visual-maps/implement-mode-map.mdc
```

### Step 3: Load Complexity-Specific Implementation Rules
Based on complexity level from `memory-bank/tasks.md`:

**Level 1:**
```
Load: .cursor/rules/isolation_rules/Level1/workflow-level1.mdc
Load: .cursor/rules/isolation_rules/Level1/optimized-workflow-level1.mdc
```

**Level 2:**
```
Load: .cursor/rules/isolation_rules/Level2/workflow-level2.mdc
```

**Level 3-4:**
```
Load: .cursor/rules/isolation_rules/Level3/implementation-intermediate.mdc
Load: .cursor/rules/isolation_rules/Level4/phased-implementation.mdc
```

## Workflow

1. **Verify Prerequisites**
   - Check `memory-bank/tasks.md` for planning completion
   - For Level 3-4: Verify creative phase documents exist
   - Review implementation plan

2. **Determine Complexity Level**
   - Read complexity level from `memory-bank/tasks.md`
   - Load appropriate workflow rules

2a. **Parallel Execution Check** (Level 2-4)
   - Check if `memory-bank/tasks.md` contains work items for parallel execution
   - If work items exist:
     - **Work Item Detection**: Identify work items ready for execution
     - **Readiness Assessment**: Find work items with:
       - Status = "Ready for Assignment" or "Unassigned"
       - All dependencies completed
       - No file conflicts with active work items
     - **Agent Spawning**: For each ready work item (up to Cursor 2.0 limit of 8 agents):
       - Assign unique work tree identifier
       - Create agent context with work item details
       - Spawn agent in isolated work tree
       - Update tasks.md: Status = "In Progress", Work Tree = [worktree-id]
     - **Parallel Execution Coordination**:
       - Monitor work item progress
       - Handle dependency resolution (unblock waiting work items when dependencies complete)
       - Detect and report conflicts
       - Update coordination status in tasks.md
     - **Completion Handling**:
       - When agent completes work item: Mark status = "Complete", prepare diff for review
       - Check if dependent work items can now start
       - When all work items complete: Coordinate integration, prepare for merge/review
   - If no work items or parallel execution not supported: Proceed with sequential workflow

3. **Execute Implementation**

   **Level 1 (Quick Bug Fix):**
   - Review bug report
   - Examine relevant code
   - Implement targeted fix
   - Write test(s) validating the fix
   - Run tests and ensure they pass
   - Update `memory-bank/tasks.md`

   **Level 2 (Simple Enhancement):**
   - If work items exist: Execute via parallel agents (see step 2a)
   - If no work items: Review build plan, examine relevant code areas, implement changes sequentially
   - Write tests for each success criterion
   - Run all tests and ensure they pass
   - Update `memory-bank/tasks.md`

   **Level 3-4 (Feature/System):**
   - If work items exist: Execute via parallel agents (see step 2a)
   - If no work items: Review plan and creative decisions, create directory structure, build in planned phases
   - **For each phase or work item:**
     - Write tests for all phase/work item success criteria
     - Run tests and ensure they pass
     - Do NOT proceed to next phase/work item until all tests pass
   - Integration testing (after all work items complete if parallel execution)
   - Document implementation
   - Update `memory-bank/tasks.md` and `memory-bank/progress.md`

4. **Test-Driven Phase Completion**
   - Extract success criteria from current phase in `memory-bank/tasks.md`
   - Write test cases covering each success criterion
   - Execute all tests
   - **Gate:** All tests MUST pass before phase completion
   - Document test results in `memory-bank/tasks.md`
   - If tests fail: fix implementation, re-run tests, repeat until all pass

5. **Command Execution**
   - Document all commands executed
   - Document results and observations
   - Follow platform-specific command guidelines

6. **Verification**
   - Verify all build steps completed
   - Verify all success criteria tests pass
   - Verify changes meet requirements
   - Update `memory-bank/tasks.md` with completion status

## Usage

Type `/build` to start implementation based on the plan in `memory-bank/tasks.md`.

## Next Steps

After implementation complete, proceed to `/reflect` command for task review.

