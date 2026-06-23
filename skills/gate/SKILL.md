---
name: gate
description: Use when about to claim work is complete, fixed, or passing, before committing or creating PRs — requires running verification commands and confirming output before making any success claims; evidence before assertions always
---

# Verification Before Completion

## Core Principle

```
NO COMPLETION CLAIMS WITHOUT FRESH VERIFICATION EVIDENCE.
Evidence before claims, always.
Violating the letter of this rule is violating the spirit of this rule.
```

## The Iron Law

```
NO COMPLETION CLAIMS WITHOUT FRESH VERIFICATION EVIDENCE
```

If you haven't run the verification command in this message, you cannot claim it passes.

## The Gate Function

```
BEFORE claiming any status or expressing satisfaction:

1. IDENTIFY: What command proves this claim?
2. RUN: Execute the FULL command (fresh, complete)
3. READ: Full output, check exit code, count failures
4. VERIFY: Does output confirm the claim?
   - If NO: State actual status with evidence
   - If YES: State claim WITH evidence
5. ONLY THEN: Make the claim

Skip any step = lying, not verifying
```

## Common Failures

| Claim | Requires | Not Sufficient |
|-------|----------|----------------|
| Tests pass | Test command output: 0 failures | Previous run, "should pass" |
| Linter clean | Linter output: 0 errors | Partial check |
| Build succeeds | Build command: exit 0 | Linter passing |
| Bug fixed | Test original symptom: passes | Code changed, assumed fixed |
| Requirements met | Line-by-line checklist | Tests passing |

## Red Flags - STOP

- Using "should", "probably", "seems to"
- Expressing satisfaction before verification
- About to commit/push/PR without verification
- Trusting agent success reports
- Relying on partial verification
- Thinking "just this once"

## Key Patterns

**Tests:**
```
[RUN test command] [SEE: 34/34 pass] "All tests pass"
```

**Build:**
```
[RUN build] [SEE: exit 0] "Build passes"
```

**Requirements:**
```
Re-read plan → Create checklist → Verify each → Report gaps or completion
```

## Quick Reference

| Dimension | Key point |
|-----------|-----------|
| Role | Rigid iron law, the mandatory gate after implementation |
| Five steps | IDENTIFY → RUN → READ → VERIFY → CLAIM |
| Forbidden | "should" / "probably" / "seems" = not verified |
| After pass | Show menu for user to choose the next flow (4 options) |
| Freshness | Previous runs don't count — must be fresh evidence from this message |

## When To Apply

**ALWAYS before:**
- ANY variation of success/completion claims
- ANY expression of satisfaction
- Committing, PR creation, task completion
- Moving to next task
- Delegating to agents

## The Bottom Line

**No shortcuts for verification.**

Run the command. Read the output. THEN claim the result.

This is non-negotiable.

## Next Step

When done:
- Verification failed → return to the current skill, fix issues, re-run verification
- Verification passed → present the following menu for the user to choose the next flow:

```
## Verification Passed ✓

How would you like to proceed?

1. Full verification — audit → inspect → ship → vault (recommended: production code, critical features)
2. Standard flow — audit → ship → vault (suitable for: internal projects, routine features)
3. Quick commit — ship → vault (suitable for: small changes, prototypes, informal projects)
4. Verify only — audit (just check the code is correct, don't commit yet)

Choose (1-4):
```

Execute the chosen flow:
- Option 1 → `tn:audit` → `tn:inspect` → `tn:ship` → `tn:vault`
- Option 2 → `tn:audit` → `tn:ship` → `tn:vault`
- Option 3 → `tn:ship` → `tn:vault`
- Option 4 → `tn:audit` (finish after done, don't auto-flow)

User requests stop → end the current flow

## Guardrails

- ALWAYS run verification before claiming completion — no exceptions
- "Should", "probably", "seems to" = not verified
- Previous runs don't count — must be fresh evidence from this message
- Partial verification is not verification
- Trusting agent reports without reading output is lying
- Violating the letter of this rule is violating the spirit of this rule
