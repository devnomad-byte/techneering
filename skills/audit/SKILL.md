---
name: audit
description: Verify implementation matches change artifacts — dual-layer verification of code vs spec compliance and code vs real requirements. Use after implementation is complete.
---

# Verify Implementation

Dual-layer verification: does the code match the spec, and does the code solve the real problem?

## Core Principle

```
NO COMPLETION WITHOUT VERIFICATION.
Every requirement traced. Every decision checked. Every gap documented.
```

## Steps

### Step 1: Select the Change

Same selection logic as tn:forge — auto-detect or ask user.

Always announce: "Verifying change: <name>"

### Step 2: Load All Artifacts

Read:
- `techneering/changes/<name>/proposal.md` — for "Why" (Layer 2)
- `techneering/changes/<name>/specs/**/*.md` — for requirements (Layer 1)
- `techneering/changes/<name>/design.md` — for architecture decisions (Layer 1)
- `techneering/changes/<name>/tasks.md` — for task completion check

### Step 3: Layer 1 — Code ↔ Spec Compliance

#### 3a. Completeness Check

- Parse `tasks.md`: count `- [ ]` (incomplete) vs `- [x]` (complete)
- If incomplete tasks exist → CRITICAL
- Extract all requirements from delta specs (`### Requirement:` headers)
- For each requirement, search codebase for implementation evidence
- If requirements appear unimplemented → CRITICAL

#### 3b. Correctness Check

- For each requirement from specs:
  - Search codebase for implementation
  - Assess if implementation matches requirement intent
  - If divergence → WARNING
- For each scenario (`#### Scenario:`):
  - Check if conditions are handled in code
  - Check if tests exist covering the scenario
  - If uncovered → WARNING

#### 3c. Coherence Check

- If design.md exists:
  - Extract key decisions
  - Verify implementation follows those decisions
  - If contradiction → WARNING
- Review new code for consistency with project patterns
- If significant deviations → SUGGESTION

### Step 4: Layer 2 — Code ↔ Real Requirements

- Read the **Why** section of proposal.md
- Trace back from the code to the original problem statement
- Ask: Does this implementation actually solve the original problem?
- Check if the "What Changes" in proposal.md are all addressed
- If the implementation solves a different problem than stated → CRITICAL

### Step 5: Generate Verification Report

```
## Verification Report: <change-name>

### Layer 1: Spec Compliance
| Dimension    | Status           |
|--------------|------------------|
| Completeness | X/Y tasks, N reqs|
| Correctness  | M/N reqs covered |
| Coherence    | Followed/Issues  |

### Layer 2: Real Requirements Check
- Original problem: <from proposal.md Why>
- Implementation addresses it: YES/NO
- Gap: <if any>

### Issues

**CRITICAL** (Must fix):
- [issue with file:line reference]

**WARNING** (Should fix):
- [issue with recommendation]

**SUGGESTION** (Nice to fix):
- [issue]

### Final Assessment
- [PASS/WARN/FAIL with reasoning]
```

## Red Flags

| Thought | Reality |
|---------|---------|
| "Tests passed, that counts as verification" | Tests verify code behavior, not whether requirements are met |
| "Spec and code roughly match is fine" | "Roughly" means divergence — must compare item by item |
| "Skip Layer 2 to save time" | Layer 2 catches "built but not what was wanted" problems |
| "Submit first even if verification fails" | Failed is failed — no "submit first" |
| "Give up after 3 failed rounds" | More than 3 rounds means a systemic problem needing human intervention |

## Quick Reference

| Dimension | Key point |
|-----------|-----------|
| Dual-layer | Layer 1: code↔spec / Layer 2: code↔real requirements |
| Issue levels | CRITICAL / WARNING / SUGGESTION |
| Rollback logic | small deviation → fix / medium → redo / fundamental → back to draft |
| Loop limit | Max 3 rounds of audit → fix, then report to user |
| Path placeholder | Always use `techneering/changes/<name>/` |

## Rollback Logic

When verify finds deviations:

| Deviation Level | Action |
|----------------|--------|
| Small (1-2 missing details) | Fix directly → re-run `tn:gate` → re-run `tn:audit` |
| Medium (wrong architecture direction) | Back to `tn:forge`, redo failed tasks → `tn:gate` → `tn:audit` |
| Fundamental (spec itself wrong) | Back to `tn:draft` to fix spec → `tn:forge` from scratch |

**Loop limit:** Max 3 rounds of verify → fix. After 3 rounds, pause and report to user.

## Next Step

When done (following the user's choice in the tn:gate menu):
- User chose full verification (1) → after passing, invoke `tn:inspect`
- User chose standard flow (2) → after passing, invoke `tn:ship`
- User chose verify only (4) → finish after done, don't auto-flow
- Verification has WARNING → fix, then re-run `tn:gate` → `tn:audit`
- Large deviation → roll back to `tn:forge`, redo failed tasks
- Spec itself wrong → roll back to `tn:draft`, fix spec docs
- User requests stop → end the current flow

## Guardrails

- Every issue must have specific file:line references and actionable recommendations
- No vague suggestions like "consider reviewing"
- When uncertain, prefer SUGGESTION over WARNING, WARNING over CRITICAL
- Graceful degradation: if only tasks.md exists, verify task completion only
- Always note which checks were skipped and why
- Unified path placeholder: always use `techneering/changes/<name>/`
