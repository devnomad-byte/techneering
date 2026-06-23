---
name: blueprint
description: Use when you have a spec or requirements for a multi-step task, before touching code
---

# Writing Implementation Plans

Write comprehensive implementation plans. Document everything needed: which files to touch, code to write, tests to run, commands to verify.

## Core Principle

```
EVERY STEP MUST CONTAIN ACTUAL CONTENT.
No placeholders. No TBD. No "fill in later". Plan failures, not task failures.
```

## Steps

### Step 1: Scope Check

If the spec covers multiple independent subsystems, suggest breaking into separate plans — one per subsystem.

### Step 2: Map File Structure

Before defining tasks, map out which files will be created or modified and what each is responsible for:
- Design units with clear boundaries and well-defined interfaces
- Prefer smaller, focused files over large ones
- Files that change together should live together
- In existing codebases, follow established patterns

### Step 3: Break Into Bite-Sized Tasks

Each step is one action (2-5 minutes):
- "Write the failing test" — step
- "Run it to make sure it fails" — step
- "Implement the minimal code to make the test pass" — step
- "Run the tests and make sure they pass" — step
- "Commit" — step

### Step 4: Write Task Details

````markdown
### Task N: [Component Name]

**Files:**
- Create: `exact/path/to/file.py`
- Modify: `exact/path/to/existing.py:123-145`
- Test: `tests/exact/path/to/test.py`

- [ ] **Step 1: Write the failing test**

```python
def test_specific_behavior():
    result = function(input)
    assert result == expected
```

- [ ] **Step 2: Run test to verify it fails**

Run: `pytest tests/path/test.py::test_name -v`
Expected: FAIL with "function not defined"

- [ ] **Step 3: Write minimal implementation**

```python
def function(input):
    return expected
```

- [ ] **Step 4: Run test to verify it passes**

Run: `pytest tests/path/test.py::test_name -v`
Expected: PASS
````

### Step 5: Self-Review

1. **Spec coverage:** Can you point to a task for each spec requirement?
2. **Placeholder scan:** Search for TBD, TODO, "implement later", "fill in details"
3. **Type consistency:** Do types and method signatures match across tasks?

Fix any issues inline.

## Red Flags

| Thought | Reality |
|---------|---------|
| "This step is too detailed" | 2-5 minutes per step is just right — coarse steps miss things |
| "Use a placeholder for now" | Placeholder = plan failure, not plan completion |
| "Similar to Task N's implementation" | Each task must be complete and standalone — can't reference other tasks |
| "Write the plan first, read spec later" | Must read all spec/design before splitting tasks |

## Quick Reference

| Dimension | Key point |
|-----------|-----------|
| Granularity | 2-5 minutes per step, one action |
| Forbidden | TBD, TODO, placeholder, "similar to Task N" |
| Files | Exact paths + line-number ranges |
| Verification | Each task includes a verify command and expected output |
| Self-check | spec coverage + placeholder scan + type consistency |

## Next Step

When done:
- Normal case → invoke `tn:assemble` to start parallel subagent execution
- Plan has issues → fix the plan and continue
- User requests stop → end the current flow

## Guardrails

- Every step must contain actual code, commands, or content — never placeholders
- These are plan failures: "TBD", "TODO", "implement later", "add appropriate error handling" (without code)
- Always map file structure before writing tasks
- Follow the spec — don't add features the spec doesn't require
- After creating the plan, tn:assemble will execute it task-by-task with subagents
- Announce at start: "I'm using the tn:blueprint skill to create the implementation plan."
