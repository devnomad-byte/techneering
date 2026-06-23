---
name: assemble
description: Use when executing implementation plans with independent tasks in the current session
---

# Assemble: Subagent-Driven Execution

Execute plans by dispatching a fresh subagent per task, with two-stage review after each: spec compliance review first, then code quality review.

## Core Principle

```
FRESH SUBAGENT PER TASK + TWO-STAGE REVIEW = HIGH QUALITY.
Never skip reviews. Never reuse context between tasks.
```

## Model Selection

Use the least powerful model that can handle each role:
- **Mechanical tasks** (1-2 files, clear specs): fast/cheap model
- **Integration tasks** (multi-file, debugging): standard model
- **Architecture/review tasks**: most capable model

## Steps

### Step 1: Read Plan and Extract Tasks

Read the implementation plan. Extract all tasks and create a todo list.

### Step 2: Execute Tasks (Per-Task Loop)

For each task:

#### 2a. Dispatch Implementer Subagent

**Frontend Detection:** Before dispatching, check if the task involves frontend/UI work. Match against the Frontend Detection Rules in `tn:compass` (keywords: page/component/UI/layout/style/React/Vue/CSS/Tailwind/form/button/animation etc.). If matched, invoke `tn:craft` first to get aesthetic guidance, then include that guidance in the implementer prompt.

Use `./implementer-prompt.md` template. Provide full task text (never make subagent read the plan file).

**One commit per task:** the implementer commits its own work before returning (the review stages below compare that commit's diff). Each task = one commit, so `2b`/`2c` reviews have a clean BASE_SHA→HEAD_SHA range per task.

Handle status:
- **DONE:** Proceed to spec compliance review
- **DONE_WITH_CONCERNS:** Read concerns, address if needed, proceed
- **NEEDS_CONTEXT:** Provide missing context and re-dispatch
- **BLOCKED:** Assess blocker, provide context or re-dispatch with better model

**Never** ignore an escalation or force the same model to retry without changes.

#### 2b. Dispatch Spec Reviewer Subagent

Use `./spec-reviewer-prompt.md` template. Verify implementer built what was requested (nothing more, nothing less).

- If issues → implementer fixes → re-review
- Don't skip the re-review

#### 2c. Dispatch Code Quality Reviewer Subagent

Use `./code-quality-reviewer-prompt.md` template. Only after spec compliance passes.

- If issues → implementer fixes → re-review
- Don't skip the re-review

#### 2d. Mark Task Complete

Mark the task as complete in the todo list and in `techneering/changes/<name>/tasks.md`.

### Step 3: Parallel Scheduling

Analyze task dependencies, build DAG:
- Independent tasks can be dispatched in parallel (max 3 concurrent)
- Dependent tasks run in topological order
- Avoid resource conflicts between parallel tasks

## Red Flags

| Thought | Reality |
|---------|---------|
| "Skip review to speed up" | Review is quality assurance, not an optional step |
| "Let the subagent read the plan file" | Must provide full text — don't make subagent read files |
| "Ignore the subagent's BLOCKED" | BLOCKED = needs your help, not "try again" |
| "Run code quality review before spec review" | Order is irreversible: spec first, then quality |

## Quick Reference

| Dimension | Key point |
|-----------|-----------|
| Flow | implementer → spec review → code quality review → next task |
| Subagents | Independent per task, don't inherit session context |
| Parallel | Independent tasks can run parallel, max 3 concurrent |
| Templates | implementer-prompt.md / spec-reviewer-prompt.md / code-quality-reviewer-prompt.md |
| Path placeholder | Always use `techneering/changes/<name>/` |

## Next Step

When done:
- Normal case (all tasks complete) → invoke `tn:gate` to run test verification
- Issue encountered → invoke `tn:diagnose` for systematic debugging
- User requests stop → end the current flow

## Guardrails

- Never skip reviews (spec compliance OR code quality)
- Never start code quality review before spec compliance passes
- Never move to next task while either review has open issues
- Never make subagent read plan file — provide full text instead
- Never ignore subagent questions or escalations
- If reviewer finds issues: implementer fixes → reviewer re-reviews → repeat until approved
- Follow tn:redgreen for each task and tn:diagnose if issues arise
