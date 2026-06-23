---
name: diagnose
description: Use when encountering any bug, test failure, or unexpected behavior, before proposing fixes
---

# Systematic Debugging

## Core Principle

```
NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST.
ALWAYS find root cause before attempting fixes. Symptom fixes are failure.
Violating the letter of this process is violating the spirit of debugging.
```

## The Iron Law

```
NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST
```

If you haven't completed Phase 1, you cannot propose fixes.

## The Four Phases

You MUST complete each phase before proceeding to the next.

### Phase 1: Root Cause Investigation

**BEFORE attempting ANY fix:**

1. **Read Error Messages Carefully**
   - Don't skip past errors or warnings
   - Read stack traces completely
   - Note line numbers, file paths, error codes

2. **Reproduce Consistently**
   - Can you trigger it reliably?
   - What are the exact steps?

3. **Check Recent Changes**
   - Git diff, recent commits
   - New dependencies, config changes

4. **Gather Evidence in Multi-Component Systems**
   - For each component boundary: log data in/out
   - Run once to gather evidence showing WHERE it breaks

5. **Trace Data Flow**
   - Where does bad value originate?
   - Keep tracing up until you find the source
   - Fix at source, not at symptom

### Phase 2: Pattern Analysis

1. Find working examples in same codebase
2. Compare against references (read COMPLETELY)
3. List every difference between working and broken
4. Understand dependencies and assumptions

### Phase 3: Hypothesis and Testing

1. **Form Single Hypothesis** — "I think X is the root cause because Y"
2. **Test Minimally** — Smallest possible change, one variable
3. **Verify Before Continuing** — Did it work?
4. **When You Don't Know** — Say "I don't understand X"

### Phase 4: Implementation

1. Create failing test case (use tn:redgreen)
2. Implement single fix
3. Verify fix
4. **If 3+ fixes failed:** Stop. Question the architecture. Discuss with user.

## Red Flags - STOP and Follow Process

- "Quick fix for now, investigate later"
- "Just try changing X and see if it works"
- Proposing solutions before tracing data flow
- "One more fix attempt" (when already tried 2+)
- Each fix reveals new problem in different place

**All of these mean: STOP. Return to Phase 1.**

## Common Rationalizations

| Excuse | Reality |
|--------|---------|
| "Issue is simple, don't need process" | Simple issues have root causes too |
| "Emergency, no time for process" | Systematic debugging is FASTER than thrashing |
| "Just try this first" | First fix sets the pattern. Do it right |
| "Multiple fixes at once saves time" | Can't isolate what worked |
| "One more fix attempt" (after 2+) | 3+ failures = architectural problem |

## Quick Reference

| Dimension | Key point |
|-----------|-----------|
| Phases | Root cause investigation → pattern analysis → hypothesis testing → implement fix |
| Iron law | No fix proposals allowed before Phase 1 is complete |
| Loop limit | 3+ failed fix attempts → stop and discuss architecture |
| Forbidden | "just try" / "quick fix" / fixing without reading error messages |
| Next step | After fix, invoke tn:redgreen to verify |

## Next Step

When done:
- Normal case → invoke `tn:redgreen`, verify the fix with red-green-refactor → after TDD, invoke `tn:gate` (show menu)
- 3+ failed fix attempts → pause, discuss architecture issues with the user
- User requests stop → end the current flow

## Guardrails

- Complete each phase before proceeding to the next — no skipping
- If 3+ fix attempts fail, stop and question the architecture
- Never propose fixes without completing Phase 1 (root cause investigation)
- Symptom fixes are failure — fix at the source, not at the symptom
- Violating the letter of this process is violating the spirit of debugging
