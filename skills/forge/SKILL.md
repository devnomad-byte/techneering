---
name: forge
description: Implement tasks from a change. Use when the user wants to start or continue implementation of an existing change.
---

# Forge a Change

Implementation entry point. Load change artifacts, then chain to `tn:blueprint` for task breakdown and `tn:assemble` for execution.

## Core Principle

```
READ ALL CONTEXT BEFORE WRITING ANY CODE.
Load every artifact. Understand the full picture. Then implement.
```

## Steps

### Step 1: Select the Change

If a name is provided, use it. Otherwise:
- Infer from conversation context if the user mentioned a change
- Auto-select if only one active change exists in `techneering/changes/`
- If ambiguous, list all active changes with progress info and ask the user to select

Always announce: "Using change: <name>"

### Step 2: Load Context

Read all artifacts from the change directory:
- `techneering/changes/<name>/proposal.md`
- `techneering/changes/<name>/specs/**/*.md`
- `techneering/changes/<name>/design.md`
- `techneering/changes/<name>/tasks.md`

Also read `techneering/config.yaml` for project context.

### Step 3: Show Current Progress

Parse `tasks.md` for checkboxes: `- [ ]` (pending) vs `- [x]` (complete).

Display:
- Change name and schema
- Progress: "N/M tasks complete"
- Remaining tasks overview

### Step 4: Create Implementation Plan (tn:blueprint)

Invoke the `tn:blueprint` skill to break down the tasks into bite-sized implementation steps:
- Exact file paths
- Complete code for each step
- Verification commands with expected output
- No placeholders

### Step 5: Execute with Assemble (tn:assemble)

Invoke the `tn:assemble` skill to execute the plan:
- Each task gets a fresh subagent
- Two-stage review per task: spec compliance → code quality
- Independent tasks can run in parallel (max 3 concurrent)
- Follow `tn:redgreen` for each task (red-green-refactor)
- Use `tn:diagnose` if issues arise

### Step 6: Track Progress

After each task completes:
- Mark the corresponding checkbox in tasks.md: `- [ ]` → `- [x]`
- Report progress: "Task N/M complete"

### Step 7: On Completion

Display:
```
## Implementation Complete

**Change:** <change-name>
**Progress:** N/N tasks complete

### Completed This Session
- [x] Task 1
- [x] Task 2
...

All tasks complete! Proceed to tn:gate to verify.
```

### Step 8: On Pause (Issue Encountered)

Display:
```
## Implementation Paused

**Change:** <change-name>
**Progress:** N/M tasks complete

### Issue Encountered
<description>

**Options:**
1. <option 1>
2. <option 2>

What would you like to do?
```

## Red Flags

| Thought | Reality |
|---------|---------|
| "I remember the requirements, no need to read files" | Lost context is the #1 cause of bugs |
| "Write code first, read spec later" | Spec is the source of truth — read it first |
| "This task is too simple for a blueprint" | All tasks go through blueprint → assemble |
| "Skip review to speed up" | Review is quality assurance — not optional |

## Quick Reference

| Dimension | Key point |
|-----------|-----------|
| Role | Implementation entry point, chains blueprint → assemble |
| Input | Change name (optional, can auto-detect) |
| Internal chain | forge → blueprint → assemble (cannot be skipped) |
| Progress tracking | Mark tasks.md checkbox after each task |
| Path placeholder | Always use `techneering/changes/<name>/` |

## Next Step

forge is a composite skill that chains internally: `tn:blueprint` → `tn:assemble`, then exits to `tn:gate`. Users do not need to trigger the intermediate steps manually.

When done:
- All tasks complete → auto-invoke `tn:gate` (then show menu)
- Issue encountered → invoke `tn:diagnose`
- User wants to pause → save progress, wait for next session
- User requests stop → end the current flow

## Guardrails

- Always read all context files before starting
- Keep code changes minimal and scoped to each task
- Update task checkbox immediately after completing each task
- Pause on errors, blockers, or unclear requirements — don't guess
- If implementation reveals design issues, pause and suggest artifact updates
- Unified path placeholder: always use `techneering/changes/<name>/`
