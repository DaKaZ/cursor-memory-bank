# REFLECT Command - Task Reflection

This command facilitates structured reflection on completed implementation, documenting lessons learned and process improvements.

## Memory Bank Integration

Reads from:
- `memory-bank/tasks.md` - Completed implementation details
- `memory-bank/progress.md` - Implementation status and observations
- `memory-bank/creative/creative-*.md` - Design decisions (Level 3-4)
- `.specstory/history/` - Agent interaction logs (if exists)

Creates:
- `memory-bank/reflection/reflection-[task_id].md` - Reflection document
- `memory-bank/reflection/interaction-summary-[task_id].md` - Human-to-agent interaction summary (if `.specstory/history` exists)
- `memory-bank/reflection/vibe-coaching-[task_id].md` - Vibe-coaching report (if `.specstory/history` exists)
- `memory-bank/reflection/improvement-suggestions-[task_id].md` - Suggested improvements to commands/rules (if `.specstory/history` exists)

Updates:
- `memory-bank/tasks.md` - Reflection status

## Progressive Rule Loading

### Step 1: Load Core Rules
```
Load: .cursor/rules/isolation_rules/main.mdc
Load: .cursor/rules/isolation_rules/Core/memory-bank-paths.mdc
```

### Step 2: Load REFLECT Mode Map
```
Load: .cursor/rules/isolation_rules/visual-maps/reflect-mode-map.mdc
```

### Step 3: Load Complexity-Specific Reflection Rules
Based on complexity level from `memory-bank/tasks.md`:

**Level 1:**
```
Load: .cursor/rules/isolation_rules/Level1/quick-documentation.mdc
```

**Level 2:**
```
Load: .cursor/rules/isolation_rules/Level2/reflection-basic.mdc
```

**Level 3:**
```
Load: .cursor/rules/isolation_rules/Level3/reflection-intermediate.mdc
```

**Level 4:**
```
Load: .cursor/rules/isolation_rules/Level4/reflection-comprehensive.mdc
```

## Workflow

1. **Verify Implementation Complete**
   - Check `memory-bank/tasks.md` for implementation completion
   - If not complete, return to `/build` command

2. **Review Implementation**
   - Compare implementation against original plan
   - Review creative phase decisions (Level 3-4)
   - Review code changes and testing

3. **Analyze Agent Interaction History (if available)**
   
   If `.specstory/history/` directory exists:
   
   a. **Determine Task Start Time**
      - Read `memory-bank/tasks.md` to identify the current task
      - Scan `.specstory/history/` files chronologically (sorted by filename timestamp)
      - Find the earliest file containing a `/van` command execution that matches the current task
      - Extract the timestamp from that specstory file's filename (format: `YYYY-MM-DD_HH-MMZ-...`)
      - Parse timestamp: `YYYY-MM-DD_HH-MMZ` represents date and time in UTC
      - This timestamp marks the start of the current task iteration
      - **Important**: Only analyze specstory files with timestamps >= this task start time
      - If no `/van` command is found in specstory files, use the earliest specstory file timestamp as fallback
   
   b. **Read Interaction Logs (Filtered by Task Start Time)**
      - Scan `.specstory/history/` for all interaction log files
      - Parse timestamps from filenames (format: `YYYY-MM-DD_HH-MMZ-...`)
      - **Filter files to only include those with timestamps >= task start time**
      - Parse human prompts and agent responses from filtered files only
      - Identify interaction patterns and sequences within the current task iteration
   
   c. **Create Interaction Summary**
      - Analyze how the agent was invoked (e.g., new feature development, bug fixes, brainstorming, refactoring, documentation)
      - Categorize interaction types and frequencies
      - Identify primary use cases and workflows
      - Document interaction patterns and sequences
      - Note the time period analyzed (from task start time to current time)
      - Create `memory-bank/reflection/interaction-summary-[task_id].md`
      - Structure: Overview (including time period), Interaction Categories, Use Case Analysis, Pattern Identification, Workflow Analysis
   
   d. **Create Vibe-Coaching Report**
      - Analyze interaction quality and effectiveness
      - Identify opportunities for more effective agent usage
      - Provide coaching on:
        - When to use specific commands (e.g., `/van`, `/plan`, `/creative`, `/build`)
        - How to structure prompts for better results
        - How to leverage Memory Bank more effectively
        - How to reduce back-and-forth cycles
        - Best practices for vibe-coding with agents
      - Highlight effective patterns observed
      - Suggest improvements to developer workflow
      - Create `memory-bank/reflection/vibe-coaching-[task_id].md`
      - Structure: Executive Summary, Effective Patterns, Improvement Opportunities, Specific Recommendations, Workflow Optimization Tips
   
   e. **Analyze for System Improvements**
      - Review all interactions for recurring patterns that indicate:
        - Missing or unclear command functionality
        - Gaps in cursor rules that required repeated explanations
        - Inefficient workflows that could be codified
        - Common misunderstandings that could be prevented
      - Apply HIGH THRESHOLD criteria:
        - Only suggest improvements for patterns that occurred MULTIPLE times
        - Focus on improvements that would prevent significant time/cycle waste
        - Prioritize changes that would prevent repeated explanations or clarifications
        - Avoid minor tweaks that could cause "drift"
      - Identify potential improvements to:
        - Command definitions and workflows
        - Cursor rules (`.cursor/rules/`)
        - Memory Bank structure or processes
        - Documentation or examples
      - Create `memory-bank/reflection/improvement-suggestions-[task_id].md`
      - Structure: Suggested Improvements, Rationale (with evidence from interactions), Impact Assessment, Implementation Notes
      - **IMPORTANT**: Do NOT automatically implement these improvements
      - Present suggestions to human for review, refinement, and confirmation
      - Wait for explicit human approval before making any changes to commands or rules

4. **Document Reflection**

   **Level 1:**
   - Quick review of bug fix
   - Document solution

   **Level 2:**
   - Review enhancement
   - Document what went well
   - Document challenges
   - Document lessons learned

   **Level 3-4:**
   - Comprehensive review of implementation
   - Compare against original plan
   - Document what went well
   - Document challenges encountered
   - Document lessons learned
   - Document process improvements
   - Document technical improvements

5. **Create Reflection Document**
   - Create `memory-bank/reflection/reflection-[task_id].md`
   - Structure: Summary, What Went Well, Challenges, Lessons Learned, Process Improvements, Technical Improvements, Next Steps
   - If interaction analysis was performed, include references to:
     - Interaction summary document
     - Vibe-coaching report
     - Improvement suggestions (if any)

6. **Present Improvement Suggestions (if applicable)**
   - If `memory-bank/reflection/improvement-suggestions-[task_id].md` was created:
     - Present the suggestions to the human developer
     - Explain the rationale and evidence for each suggestion
     - Wait for human review and refinement
     - Only implement changes after explicit human confirmation
     - Document which suggestions were approved and implemented

7. **Update Memory Bank**
   - Update `memory-bank/tasks.md` with reflection status
   - Mark reflection phase as complete
   - If interaction analysis was performed, note the additional documents created

## Interaction Analysis Details

### Interaction Summary Structure

The interaction summary document (`interaction-summary-[task_id].md`) should include:

```markdown
# Interaction Summary: [Task Name]

## Overview
- Total interactions analyzed: [count]
- Time period: [task start timestamp] to [current timestamp]
- Task start identified: [timestamp from specstory file containing `/van` command]
- Specstory files analyzed: [list of filenames with timestamps >= task start]
- Primary interaction types: [list]

## Interaction Categories
- **Feature Development**: [count] interactions
- **Bug Fixes**: [count] interactions
- **Brainstorming/Exploration**: [count] interactions
- **Refactoring**: [count] interactions
- **Documentation**: [count] interactions
- **Other**: [count] interactions

## Use Case Analysis
[Detailed analysis of how the agent was used for different purposes]

## Pattern Identification
[Common patterns in how prompts were structured and agent was invoked]

## Workflow Analysis
[Analysis of the development workflow and how it evolved]
```

### Vibe-Coaching Report Structure

The vibe-coaching report (`vibe-coaching-[task_id].md`) should include:

```markdown
# Vibe-Coaching Report: [Task Name]

## Executive Summary
[High-level overview of coaching insights]

## Effective Patterns Observed
- [Pattern 1]: [Description and why it worked well]
- [Pattern 2]: [Description and why it worked well]

## Improvement Opportunities
- [Opportunity 1]: [Description and potential impact]
- [Opportunity 2]: [Description and potential impact]

## Specific Recommendations
1. **Command Usage**: [Recommendations on when/how to use commands]
2. **Prompt Structure**: [Tips for structuring effective prompts]
3. **Memory Bank Usage**: [How to better leverage Memory Bank]
4. **Workflow Optimization**: [Suggestions for streamlining workflow]

## Workflow Optimization Tips
[Actionable tips for more effective vibe-coding]
```

### Improvement Suggestions Structure

The improvement suggestions document (`improvement-suggestions-[task_id].md`) should include:

```markdown
# Improvement Suggestions: [Task Name]

## Suggested Improvements

### 1. [Improvement Title]
- **Type**: [Command/Rule/Documentation/Process]
- **Rationale**: [Why this improvement is needed, with evidence from interactions]
- **Evidence**: 
  - [Specific example 1 from interactions]
  - [Specific example 2 from interactions]
  - [Frequency/pattern observed]
- **Impact Assessment**: 
  - **High/Medium/Low** impact
  - Expected reduction in cycles/time
  - Expected improvement in clarity/efficiency
- **Implementation Notes**: [How this could be implemented]
- **Risk Assessment**: [Potential risks or downsides]

### 2. [Improvement Title]
[...]

## Summary
[Summary of all suggestions and overall impact if implemented]
```

### High Threshold Criteria for Improvements

Only suggest improvements that meet ALL of the following criteria:

1. **Recurring Pattern**: The issue/opportunity appeared in MULTIPLE interactions (minimum 3+ occurrences)
2. **Significant Impact**: Would prevent substantial time/cycle waste or repeated explanations
3. **Clear Solution**: The improvement is well-defined and implementable
4. **Prevents Drift**: The improvement codifies important learning rather than making minor tweaks
5. **Evidence-Based**: Clear evidence from interaction logs supports the need

Examples of improvements that WOULD meet the threshold:
- Multiple cycles spent explaining integration testing → Add integration testing guidance to a command or rule
- Repeated confusion about when to use `/plan` vs `/creative` → Enhance command documentation with clear decision criteria
- Frequent need to re-explain project structure → Add project structure context to Memory Bank initialization

Examples of improvements that would NOT meet the threshold:
- Single instance of a minor clarification needed
- Preference-based suggestions without clear evidence
- Minor wording improvements to existing rules
- One-off issues that are unlikely to recur

## Usage

Type `/reflect` to start reflection on the completed task.

If `.specstory/history/` exists, the command will automatically:
1. Determine the task start time by finding the `/van` command that initiated the current task
2. Filter specstory history files to only include those from the task start time onwards
3. Analyze the filtered interaction history (current task iteration only)
4. Create interaction summary and vibe-coaching report
5. Analyze for system improvements (with high threshold)
6. Present improvement suggestions for your review and confirmation

**Note**: The analysis is scoped to the current task iteration only. Files from previous tasks are excluded to provide focused insights on the current work cycle.

## Next Steps

After reflection complete:
- Review improvement suggestions (if any) and confirm which to implement
- Proceed to `/archive` command to finalize task documentation

